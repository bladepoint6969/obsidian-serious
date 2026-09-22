# audit2allow

`audit2allow` generates [[SELinux]] policy rules from denial messages in the audit log. When a denial has no matching boolean or label fix, it can translate the recorded AVC (Access Vector Cache) denials into a loadable policy module that permits exactly what was blocked.

A typical invocation pipes recent denials into it and builds a module:

```bash
sudo ausearch -m avc -ts recent | audit2allow -M mymodule
sudo semodule -i mymodule.pp
```

The first command produces `mymodule.pp` (a compiled policy package) along with a `mymodule.te` source file you can review; the second installs it.

Generating a custom module should be a last resort, used only after understanding why the denial happened -- never blindly. A denial can be a legitimate signal of an attack, and blindly allowing it defeats the point of [[Mandatory Access Control]]. Prefer flipping a boolean or correcting a label with [[semanage]] and [[restorecon]] first; reach for `audit2allow` only when no cleaner fix exists. The [[sealert]] output often names the appropriate fix before it comes to this.

#linux #security
