# sealert

`sealert` is the [[SELinux]] diagnostic command that turns cryptic denial messages into plain-language explanations and suggested fixes. It is part of the [[setroubleshoot]] toolset, which analyses AVC (Access Vector Cache) denials and often tells you exactly which boolean to flip or which command to run.

Run it against the audit log to analyse recorded denials:

```bash
sudo sealert -a /var/log/audit/audit.log
```

The output describes what was denied, offers a likely cause, and recommends a remedy -- frequently a specific `setsebool` command or a labelling fix using [[semanage]] and [[restorecon]]. When it suggests generating a custom policy module, that points toward [[audit2allow]], which should still be treated as a last resort.

`sealert` sits at step three of the usual SELinux troubleshooting workflow: after confirming SELinux is the cause and reading the raw denials, use it to get a human-readable explanation before applying the right fix.

#linux #security
