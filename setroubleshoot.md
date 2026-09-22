# setroubleshoot

`setroubleshoot` is the [[SELinux]] troubleshooting toolset that monitors audit logs for denials and translates them into human-readable explanations with suggested fixes. It is the machinery behind the plain-language guidance you get when diagnosing SELinux problems, and it exposes the [[sealert]] command as its main front end.

When a denial occurs, setroubleshoot analyses the AVC (Access Vector Cache) message and produces an explanation of what was blocked, a probable cause, and a recommended remedy -- often a specific boolean to flip or a labelling command to run. These alerts are also surfaced through the system journal:

```bash
sudo journalctl -t setroubleshoot
```

Interactively, you query its analysis with `sealert`, for example `sudo sealert -a /var/log/audit/audit.log`. Its recommendations typically point toward the cleanest fix first -- a boolean, or a label correction with [[semanage]] and [[restorecon]] -- and only toward generating a custom module with [[audit2allow]] as a last resort.

#linux #security
