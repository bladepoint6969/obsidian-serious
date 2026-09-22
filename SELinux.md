# SELinux

SELinux (Security-Enhanced Linux) is a [[Mandatory Access Control|mandatory access control (MAC)]] system built into the Linux kernel. To understand what that means and why it matters, it helps to start with what it replaces or supplements.

## The core idea: MAC vs DAC

Standard Linux permissions use [[Discretionary Access Control]] (DAC) -- the `rwx` bits and ownership you set with `chmod` and `chown`. It is called "discretionary" because the owner of a resource has discretion to grant access to it. If you own a file, you can `chmod 777` it and hand access to everyone.

DAC has a fundamental weakness: it trusts processes to behave. If a web server runs as user `apache`, it can access anything `apache` can access. If an attacker compromises that web server, they inherit all of `apache`'s permissions -- they can read every file `apache` owns, and potentially anything world-readable like `/etc/passwd`.

[[Mandatory Access Control]] (MAC) adds a second layer that the process owner cannot override. Rules are set centrally by policy, and even root is constrained by them. The kernel checks both DAC and MAC: an operation must pass DAC first, then SELinux gets a veto.

The mental model: DAC asks "does this user own this / have the bits?" SELinux asks "does policy explicitly allow this specific process type to do this specific thing to this specific resource type?" If policy does not say yes, the answer is no (default-deny).

## Labels are everything

SELinux does not reason about users, filenames, or paths the way DAC does. Instead, every subject (process) and object (file, port, socket, device) gets a security context -- a label. Access decisions are made by comparing labels against policy rules.

A context looks like this:

```text
system_u:object_r:httpd_sys_content_t:s0
```

Those four fields are:

- user (`system_u`) -- the SELinux user (not the same as a Linux user)
- role (`object_r`) -- used mainly for [[Role-Based Access Control|RBAC]] and process transitions
- type (`httpd_sys_content_t`) -- the field that matters most in practice
- level (`s0`) -- for [[Multi-Level Security|MLS/MCS]] (multi-level/category security), often ignorable

In everyday use, 99% of SELinux troubleshooting is about the type field. This model is called [[Type Enforcement]] (TE).

You can see labels everywhere with the `-Z` flag:

```bash
ls -Z /var/www/html          # file labels
ps -eZ | grep httpd          # process labels
id -Z                        # your shell's context
netstat -Z / ss -Z           # socket labels
```

## Type Enforcement in practice

The rule structure is essentially:

> A process of type X (a domain) may perform action Y on an object of type Z.

Concrete example with a web server:

- The Apache process runs in the domain `httpd_t`.
- Web content in `/var/www/html` is labeled `httpd_sys_content_t`.
- Policy contains a rule: `httpd_t` may read `httpd_sys_content_t`.

So Apache can serve your web pages. But now consider an attack scenario:

- An attacker exploits Apache and tries to read `/etc/shadow` (labeled `shadow_t`).
- There is no rule allowing `httpd_t` to read `shadow_t`.
- The kernel denies it -- even though the exploit is running as a process that DAC might otherwise permit.

This is the whole value proposition: SELinux confines a compromised service to only what its job requires. This is why moving web content outside `/var/www` breaks things -- the new location has the wrong label, and `httpd_t` is not allowed to read it, regardless of `chmod 777`.

## The three modes

SELinux runs in one of three modes, checked with `getenforce`:

- Enforcing -- policy is applied; denials are blocked and logged.
- Permissive -- policy is not enforced, but denials are still logged. Extremely useful for debugging: you see everything that would have been blocked without breaking anything.
- Disabled -- SELinux is off entirely (and labels stop being maintained).

```bash
getenforce                   # show current mode
sudo setenforce 0            # switch to permissive (temporary)
sudo setenforce 1            # switch to enforcing (temporary)
```

Permanent mode lives in `/etc/selinux/config`. Note: switching from disabled to enabled requires a filesystem relabel (and a reboot), because labels were not being tracked while it was off.

## Booleans: policy tunables

Writing policy is hard, so distributions ship booleans -- on/off switches that toggle common policy decisions without editing raw rules. For example, by default Apache cannot make outbound network connections (a common exfiltration/SSRF vector). If your app legitimately needs that:

```bash
getsebool -a | grep httpd            # list httpd-related booleans
sudo setsebool -P httpd_can_network_connect on
```

The `-P` makes it persistent across reboots. Booleans cover a huge fraction of real-world "I need to allow this specific thing" cases.

## The practical troubleshooting workflow

This is what actually matters day to day. When something breaks and you suspect SELinux:

1. Confirm it is SELinux. Temporarily set permissive (`sudo setenforce 0`) and retry. If it works now, SELinux was the cause. Set it back to enforcing afterward.
2. Read the denial logs. Denials are recorded as AVC (Access Vector Cache) messages:

   ```bash
   sudo ausearch -m avc -ts recent
   sudo journalctl -t setroubleshoot
   ```

3. Get a human-readable explanation and suggested fix. The [[setroubleshoot]] tools translate cryptic denials into plain guidance:

   ```bash
   sudo sealert -a /var/log/audit/audit.log
   ```

   This will often tell you exactly which boolean to flip or which command to run.

4. Apply the right fix, in order of preference:
   - Flip a boolean if one exists -- cleanest option.
   - Fix the label if a file is in the wrong place or mislabeled:

     ```bash
     # Add a persistent rule for a custom location...
     sudo semanage fcontext -a -t httpd_sys_content_t "/srv/web(/.*)?"
     # ...then apply it
     sudo restorecon -Rv /srv/web
     ```

     `restorecon` resets labels to what policy says they should be. `chcon` changes a label directly but does not survive a relabel, so prefer `semanage fcontext` + `restorecon` for anything permanent.
   - Generate a custom policy module as a last resort, only after understanding why the denial happened (never blindly -- a denial can be a real attack):

     ```bash
     sudo ausearch -m avc -ts recent | audit2allow -M mymodule
     sudo semodule -i mymodule.pp
     ```

## Key mental shifts for someone coming from DAC

- Paths do not matter, labels do. Copying vs. moving a file changes behavior: `cp` inherits the destination's label, `mv` preserves the original label. This trips people up constantly.
- Root is not all-powerful. A confined process running as root is still bound by policy.
- Default-deny. If no rule permits an action, it is blocked. Silence in policy means "no."
- Do not disable it to fix a problem. Setting permissive to diagnose is fine; disabling entirely throws away the protection. The right fix is almost always a boolean or a label correction.

## The essential toolkit

| Command | Purpose |
| --- | --- |
| `getenforce` / `setenforce` | Check / change mode |
| `ls -Z`, `ps -Z`, `id -Z` | View labels/contexts |
| `getsebool -a` / `setsebool -P` | List / set booleans |
| [[semanage]] `fcontext` | Define persistent file label rules |
| [[restorecon]] `-Rv` | Apply correct labels to files |
| `chcon` | Change a label directly (non-persistent) |
| `ausearch -m avc` | Find denial events |
| [[sealert]] `-a` | Explain denials in plain language |
| [[audit2allow]] | Generate policy from denials |

---

The one-sentence summary: SELinux labels every process and resource, then enforces a central, default-deny policy that says which process types may act on which resource types -- so that even a fully compromised service can only do what its job explicitly requires.

#linux #security
