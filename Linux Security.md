# Linux Security

Map of Content for Linux security topics -- a hub linking out to the access-control models, mechanisms, and tools collected in this vault. Start here to navigate the cluster rather than hunting through folders.

## Access control models

- [[Discretionary Access Control]] -- the standard owner-and-permission-bits model (`rwx`, `chmod`, `chown`).
- [[Mandatory Access Control]] -- centrally enforced policy that owners and root cannot override.
- [[Role-Based Access Control]] -- permissions assigned through roles rather than to individuals.
- [[Multi-Level Security]] -- classification levels and categories (MLS/MCS) controlling information flow.

## SELinux

- [[SELinux]] -- Security-Enhanced Linux, the kernel's MAC implementation and the anchor for this cluster.
- [[Type Enforcement]] -- the label-and-type mechanism SELinux uses for most access decisions.

## SELinux tooling

- [[semanage]] -- manage persistent policy customisations (file contexts, ports, booleans).
- [[restorecon]] -- reset file labels to what policy defines.
- [[setroubleshoot]] -- toolset that translates denials into plain-language guidance.
- [[sealert]] -- query setroubleshoot's analysis of denials.
- [[audit2allow]] -- generate a custom policy module from denials (last resort).

## All notes in this cluster

```dataview
LIST
FROM (#linux AND #security) AND !"templates"
WHERE file.name != this.file.name
SORT file.name ASC
```

#linux #security #moc
