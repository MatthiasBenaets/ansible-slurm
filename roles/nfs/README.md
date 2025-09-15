# NFS

This role sets up **NFS shares** on the controller node and mounts them on the compute nodes.
It ensures that home directories are accessible across all nodes in the cluster.
It's will also be used to set up scratch spaces, and Lmod module directories

## Summary of Tasks

**Controller node tasks:**

- Install NFS server packages (`nfs-kernel-server`)
- Create shared directory for home
- Configure `/etc/exports` and export NFS shares

**Compute/login node tasks:**

- Install NFS client packages (`nfs-common`)
- Create mount points for shared directory
- Mount NFS shares from the controller node

## Available Tags

- `nfs` - Run all NFS-related tasks
- `nfs-controller` - Tasks specific to the NFS server (controller)
- `nfs-node` - Tasks specific to NFS clients (compute/login nodes)
