# Role-Based Access Control

Role-Based Access Control (RBAC) is a model in which permissions are assigned to roles, and roles are assigned to users, rather than granting permissions directly to individuals. A user acquires the permissions of whatever role they hold, which makes access easier to reason about and administer as the number of users and permissions grows.

In [[SELinux]], the role is one of the four fields in a security context (for example, `object_r` in `system_u:object_r:httpd_sys_content_t:s0`). Roles are used mainly to constrain which domains a user may enter and to govern process transitions -- that is, which types a subject in a given role is allowed to run as. This complements [[Type Enforcement]], which handles the bulk of everyday access decisions through the type field.

RBAC is a general access-control concept that appears well beyond SELinux -- in operating systems, databases, and cloud IAM systems -- wherever administrators prefer to manage permissions through named roles instead of per-user grants. It is one of several models that can sit under a broader [[Mandatory Access Control]] policy.

#linux #security
