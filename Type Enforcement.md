# Type Enforcement

Type Enforcement (TE) is the core mechanism [[SELinux]] uses to make access decisions. Every subject (a process) and object (a file, port, socket, device) carries a security context whose most important field is its type. Policy is then expressed as rules over these types, and the kernel allows an operation only if a rule explicitly permits it (default-deny).

The rule structure is essentially: a process of type X (a domain) may perform action Y on an object of type Z. For example, the Apache process runs in the domain `httpd_t`, web content in `/var/www/html` is labelled `httpd_sys_content_t`, and a policy rule grants `httpd_t` read access to `httpd_sys_content_t` -- so Apache can serve those pages. There is no rule letting `httpd_t` read `shadow_t`, so a compromised Apache cannot read `/etc/shadow`, even if DAC would otherwise allow it.

Because decisions are made from labels rather than paths, type enforcement is why file location and labelling matter so much in SELinux. Moving content to an unlabelled location, or copying it so it inherits the wrong label, can break access regardless of the `rwx` bits.

Type Enforcement is SELinux's primary expression of [[Mandatory Access Control]]. It works alongside the context's other fields, which support [[Role-Based Access Control|RBAC]] and [[Multi-Level Security|MLS/MCS]], though in everyday troubleshooting the type field is what matters most.

#linux/selinux #security
