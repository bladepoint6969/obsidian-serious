# Mandatory Access Control

Mandatory Access Control (MAC) is a security model in which access rules are set centrally by a policy that individual users and processes cannot override -- not even the resource owner, and not even root. The system enforces the policy uniformly, so a subject's access to an object is decided by rules the subject has no discretion to change.

This is the defining contrast with [[Discretionary Access Control]] (DAC), where the owner of a resource has the discretion to grant access to it (for example, `chmod 777` on a file you own). Under MAC, that discretion is removed: even if DAC would permit an operation, the policy can still deny it. In practice the two layers stack -- an operation must pass DAC first, and then the MAC layer gets a veto (default-deny: if no rule permits an action, it is blocked).

MAC is the model that [[SELinux]] implements in the Linux kernel. Other implementations include AppArmor on Linux and MAC schemes in hardened and multi-level secure operating systems. SELinux's particular flavour of MAC is built on labels and [[Type Enforcement]], with additional support for [[Role-Based Access Control|RBAC]] and [[Multi-Level Security|MLS/MCS]].

The value proposition: because the policy confines a process to only what its job requires, a compromised service can only do what the policy explicitly allows, regardless of what the underlying user account could otherwise reach.

#linux #security
