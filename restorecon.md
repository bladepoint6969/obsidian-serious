# restorecon

`restorecon` resets the [[SELinux]] security labels on files to the values defined by policy. Rather than setting a label you specify, it looks up what the policy says a path should be labelled and applies that, which makes it the safe, repeatable way to fix mislabelled files.

It is typically used right after declaring a file-context rule with [[semanage]] `fcontext`, to apply that rule to files already on disk:

```bash
# Recursively relabel a tree, showing what changes
sudo restorecon -Rv /srv/web
```

The `-R` recurses into directories and `-v` reports each change. Because `restorecon` derives labels from policy, it is preferred over `chcon` for anything permanent: `chcon` sets a label directly but does not survive a filesystem relabel, whereas the `semanage fcontext` + `restorecon` combination re-applies correctly every time.

This tool is central to the SELinux labelling workflow, and a full relabel is also what is required when switching SELinux from disabled back to enforcing, since labels are not maintained while it is off.

#linux/selinux #security
