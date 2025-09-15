# Hostname

This role sets the correct **hostname** for each node and ensures all cluster nodes are listed in `/etc/hosts`.
It is only required if hostnames are not managed network-wide. Correct hostnames are critical for proper SLURM cluster operation.

## Summary of Tasks

- Set the system hostname based on the inventory
- Update `/etc/hosts` to include all cluster nodes

## Available Tags

- `hostname` - Configure hostnames and `/etc/hosts`
