# Multi-Level Security

Multi-Level Security (MLS) is an access-control model that classifies subjects and objects by sensitivity level and controls information flow between levels. Its classic rules come from the Bell-LaPadula model: no read up (a subject cannot read data above its clearance) and no write down (a subject cannot write data to a level below its own), which together prevent sensitive information from leaking to lower levels. It originates in environments with formal classification schemes, such as government and military systems.

In [[SELinux]], MLS corresponds to the level field of a security context -- the `s0` in `system_u:object_r:httpd_sys_content_t:s0`. Most general-purpose systems can ignore this field, since everyday access is decided by [[Type Enforcement]] on the type field. MLS becomes relevant when a deployment genuinely needs to separate data by classification.

Multi-Category Security (MCS) is a simpler, more widely used variant built on the same machinery. Instead of hierarchical clearance levels, MCS attaches arbitrary categories to processes and files and requires them to match for access. It is the mechanism behind container and virtual-machine isolation on Linux (for example, giving each container a distinct category so it cannot touch another's files), which is why MLS/MCS is often written together.

Both are refinements layered on top of SELinux's core [[Mandatory Access Control]] policy.

#security
