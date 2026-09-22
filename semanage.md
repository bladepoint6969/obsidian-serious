# semanage

`semanage` is the [[SELinux]] policy management command used to make persistent changes to policy configuration without editing or compiling raw policy source. It manages the local customisations that survive reboots and filesystem relabels.

Its most common use is defining persistent file-context rules -- telling policy what label a path should have -- via the `fcontext` subcommand:

```bash
# Declare that files under /srv/web should be labelled httpd_sys_content_t
sudo semanage fcontext -a -t httpd_sys_content_t "/srv/web(/.*)?"
```

This only records the rule; it does not relabel existing files. Apply it to files on disk with [[restorecon]], which resets labels to what policy says they should be. This `semanage fcontext` + `restorecon` pairing is the preferred way to label custom locations permanently, in contrast to `chcon`, which changes a label directly but does not survive a relabel.

`semanage` manages much more than file contexts -- ports, booleans, users, logins, and interfaces among them (for example, `semanage port -a` to permit a service to bind a non-standard port). It is a central tool in the SELinux troubleshooting workflow when the right fix is a label or policy customisation rather than a boolean.

#linux/selinux #security
