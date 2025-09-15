# SLURM

This role sets up a **SLURM cluster** including the controller, compute nodes, and optional REST API and accounting database.
It also integrates **OpenMPI** and configures a custom **MOTD** for login nodes.

## Summary of Tasks

**Controller node tasks:**

- Install SLURM packages (`slurm-wlm`, `munge`, etc.)
- Configure `slurmctld`, `slurmdbd` (optional), and `slurmrestd` (optional)
- Generate and manage `munge.key`
- Configure SLURM spool and log directories
- Configure OpenMPI
- Create systemd services and restart necessary daemons

**Compute/login node tasks:**

- Install SLURM client packages
- Fetch `munge.key` from controller
- Deploy `slurm.conf` and `cgroup.conf`
- Set up `slurmd` daemon
- Configure OpenMPI
- Mount shared directories if needed

**Optional tasks:**

- Set up SLURM accounting database (`slurmdbd`) with MariaDB
- Configure SLURM REST API (`slurmrestd`)
- Create custom MOTD for login nodes showing SLURM info

## Available Tags

- `slurm` - General SLURM installation
- `slurm-controller` - Tasks specific to the controller node
- `slurmdbd-controller` - Accounting database setup
- `slurmrestd-controller` - REST API setup
- `slurm-node` - Tasks specific to compute/login nodes
- `openmpi` - Install and configure OpenMPI
- `motd` - Configure custom login messages
