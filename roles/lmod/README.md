# Lmod

This role sets up **Lmod**, a module system for managing software packages and environment modules.
It helps maintain a clean user environment and simplifies version management for cluster software.

## Summary of Tasks

**Controller node tasks:**

- Install the Lmod package
- Create shared directories for applications and modulefiles
- Configure `/etc/profile.d/lmod.sh` to update `MODULEPATH`
- Export shared directories via NFS

**Compute/login node tasks:**

- Install the Lmod package
- Mount shared directories from the controller
- Update `MODULEPATH` for user sessions

## Available Tags

- `lmod` - Install Lmod and configure environment
- `lmod-controller` - Tasks specific to the controller node
- `lmod-node` - Tasks specific to compute/login nodes
