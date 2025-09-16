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

## Notes

### CLI

Slurm provides many CLI commands and tools, which can be used to manage the cluster. For more information, refer to the [Slurm documentation](https://slurm.schedmd.com/).\
Below are some useful commands:

- `sinfo` - Show information about available partitions and nodes
- `squeue` - Show information about jobs in the queue
- `sacct` - Show accounting information for jobs
- `scontrol` - Control SLURM daemons
- `srun` - Run jobs on the cluster
- `sbatch` - Submit batch jobs to the cluster
- `scancel` - Cancel jobs on the cluster

### Node status

The `sinfo` command can be used to check the status of nodes in the cluster.\
Each node will have a status. Available nodes often are in `idle`, `alloc`, `mix`.\
If a node has status `inval`, it's recommended to check the slurmctld system service logs. It's often a `slurm.conf` issue.\
Nodes can also have the status `drain`. If this was not intentional, it's possible the node unexpectedly rebooted. Check the node in this case. \
To get it running again, run `scontrol update nodename=<nodename> state=resume`.

### Multiple node types

If you have multiple nodes with different specifications in the same partition, you should specify it in the `slurm.conf` file.\
Here is an example:

```
NodeName=node[1-2] CPUs=2 RealMemory=1900 Sockets=1 CoresPerSocket=2 ThreadsPerCore=1 State=UNKNOWN
NodeName=node[3-4] CPUs=4 RealMemory=8000 Sockets=1 CoresPerSocket=4 ThreadsPerCore=1 State=UNKNOWN

PartitionName=debug Nodes=node[1-4] Default=YES MaxTime=INFINITE State=UP
```

If you prefer splitting up nodes into separate partitions, you can set something up like below.\
By use `Default=YES`, all jobs without a specific partition will be assigned to, in this example, the `lowmem` partition.\
For example with sbatch, you can specify the partition with `sbatch --partition=highmem script.sh`

```
NodeName=node[1-2] CPUs=2 RealMemory=1900 Sockets=1 CoresPerSocket=2 ThreadsPerCore=1 State=UNKNOWN

NodeName=node[3-4] CPUs=4 RealMemory=8000 Sockets=1 CoresPerSocket=4 ThreadsPerCore=1 State=UNKNOWN

PartitionName=lowmem Nodes=node[1-2] Default=YES MaxTime=INFINITE State=UP

PartitionName=highmem Nodes=node[3-4] Default=NO MaxTime=INFINITE State=UP
```

### Cgroup

Slurm is currently configured to use cgroups to control memory and CPU usage.\
More information about cgroups can be found [here](https://slurm.schedmd.com/cgroup.conf.html#SECTION_EXAMPLE).\
To check if cgroups are working as intended, you can run the following commands:

```bash
sbatch --wrap="sleep 300"
squeue -j <job_number>
ssh <job_node>
ps -u $USER | grep sleep
cat /proc/<id>/cgroup
```

### SSH login

Something that has not been implemented in this playbook, but can be set up is only allowing ssh into Slurm jobs on the compute nodes.\
The follow steps need to be taken to implement this:

1. Install `libpam-slurm-adopt`
2. Edit `/etc/pam.d/sshd`
3. Append `account  required pam_slurm_adopt.so`, and `session  required pam_slurm_adopt.so` to the end of the file
4. Restart sshd via `systemctl restart sshd`

**Important**: You will be kicked out of the ssh session and you will only be able to connect when an active job is running.\
To ensure that the admin account can still access the nodes, add the following settings as well to the file.

```bash
account    [success=1 default=ignore] pam_succeed_if.so user in root ansible slurm
account    required     pam_slurm_adopt.so
session    [success=1 default=ignore] pam_succeed_if.so user in root ansible slurm
session    required     pam_slurm_adopt.so
```

### Slurm REST API

If you plan on using a custom dashboard (instead of Open OnDemands), you might want to retrieve data from the Slurm server.\
This requires the `slurmrestd` service to be running and correctly set up.\
This should already be set up using the `slurmrestd-controller.yml` file.

To actually be able to use the API, you will need to generate a token.\
This can be done by running `scontrol token username=$USER lifespan=86400`.\
Obviously, feel free to use whichever user and lifespan you want.

You can now access the API. To test this, you can use `curl`.

```bash
curl -X GET
-H "X-SLURM-USER-TOKEN:the_token_you_just_generated" 'http://localhost:6820/slurm/v0.0.40/nodes'
```

Note that any API call that is pointed at `/slurmdb` won't work.
To get access, you will need to create an account using `sacctmgr`.

```bash
sacctmgr create account name=$USER description="User Account" organisation="My Org"
# or with privileges such as Operator or Admin
sacctmgr create account name=$USER description="User Account" organisation="My Org" AdminLevel=Admin
# to list users and account:
sacctmgr list Accounts
sacctmgr list Users
```

You can also just generate a token using `sudo` and use that instead.
