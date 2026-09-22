# Discretionary Access Control

Discretionary Access Control (DAC) is the standard permission model on Unix-like systems: access to a resource is governed by its owner and the permission bits set on it. It is called "discretionary" because the owner has the discretion to grant access -- if you own a file, you can change its mode and ownership freely, including handing access to everyone with `chmod 777`.

On Linux, DAC is the familiar `rwx` bits and ownership managed with `chmod` and `chown`. Access decisions are made from the acting user's identity and the object's owner/group/other permissions.

DAC's fundamental weakness is that it trusts processes to behave, and access follows the user identity. If a service runs as user `apache`, it can reach anything `apache` can reach; compromise the service and the attacker inherits all of that account's permissions, including world-readable files like `/etc/passwd`.

This is precisely the gap that [[Mandatory Access Control]] (MAC) closes. Systems like [[SELinux]] layer a MAC policy on top of DAC: an operation must first pass the DAC check, and then the MAC layer can still deny it based on process and resource labels rather than user identity. DAC asks "does this user own this / have the bits?"; MAC asks "does policy allow this process type to do this to this resource type?"

#security
