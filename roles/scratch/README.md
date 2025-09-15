# Scratch

This role sets up a **shared scratch directory** accessible by all users.
Unlike home directories, the scratch directory has **no quota limits** and is intended for temporary or large data files.
Files in this directory are automatically **purged after a configurable number of days**.

## Summary of Tasks

**Controller node tasks:**

- Install NFS server packages (`nfs-kernel-server`) if required
- Create the shared scratch directory with proper permissions
- Configure `/etc/exports` to share the directory via NFS
- Create a purge script and configure a daily cron job to remove old files

**Compute/login node tasks:**

- Install NFS client packages (`nfs-common`)
- Create local mount point if it doesn’t exist
- Mount the scratch NFS share from the controller node

## Available Tags

- `scratch` - Run all scratch-related tasks
- `scratch-controller` - Tasks specific to the controller node
- `scratch-node` - Tasks specific to compute/login nodes
