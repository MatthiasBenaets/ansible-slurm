# Ansible-SLURM

This repository contains an **Ansible playbook for deploying a SLURM cluster**.
It installs all required packages, services, and configuration files, providing a fully automated cluster setup.

The playbook is organized into **roles**, located in the [roles](roles) directory.
Roles can be selectively executed using **tags**, and each role includes its own `README.md` with additional details.

The playbook was originally designed to provide a single-command deployment for potential SIMLAB nodes, but it is fully customizable for other SLURM clusters as well.

---

## Roles

| Role                               | Description                                                           |
| ---------------------------------- | --------------------------------------------------------------------- |
| [common](roles/common)             | Common dependencies and configuration                                 |
| [timedate](roles/timedate)         | Set system time and date                                              |
| [hostname](roles/hostname)         | Configure hostname and `/etc/hosts`                                   |
| [smtp](roles/smtp)                 | Mail server configuration                                             |
| [ad](roles/ad)                     | Active Directory authentication                                       |
| [ldap](roles/ldap)                 | OpenLDAP authentication                                               |
| [nfs](roles/nfs)                   | NFS server and client setup                                           |
| [scratch](roles/scratch)           | Create scratch directories                                            |
| [quota](roles/quota)               | User quotas management                                                |
| [lmod](roles/lmod)                 | Lmod environment modules setup                                        |
| [slurm](roles/slurm)               | SLURM cluster installation and configuration                          |
| [openondemand](roles/openondemand) | Open OnDemand configuration for user-friendly SLURM access            |
| [package](roles/package)           | Install additional software packages (Python, R, RStudio, JupyterLab) |

_Please follow the link for more information about each role._

---

## Usage

1. **Edit your hosts file** to define the cluster nodes, ideally using FQDNs.
2. **Edit group variables** in [group_vars/all.yml](group_vars/all.yml) or create a custom vars file.
3. **Run the playbook**:

_Example using OpenLDAP and various software packages:_

```bash
ansible-playbook \
  --ask-become-pass \
  --inventory hosts \
  --extra-vars '{
    "domain_name": "example.com",
    "postfix_server_name": "example.com",
    "mail_username": "secret@example.com",
    "mail_password": "secret",
    "relay_host": "[smtp.gmail.com]:587",
    "auth_backend": "ldap",
    "ldap_password": "secret",
    "slurm_compute_nodes": [
      "NodeName=node[1-2].example.com CPUs=2 RealMemory=1900 Sockets=1 CoresPerSocket=2 ThreadsPerCore=1 State=UNKNOWN",
      "PartitionName=debug Nodes=ALL Default=YES MaxTime=INFINITE State=UP"
    ],
    "slurm_acct": true,
    "mysql_password": "secret",
    "slurmdb_password": "secret",
    "slurm_restapi": true,
    "python_version": "3.12.0",
    "jupyterlab_version": "4.4.6",
    "r_version": "4.5.0",
    "rstudio_version": "v2023.05.1+513"
  }' \
  site.yml
```

You can also override group variables or reference a custom vars file instead of passing all variables on the command line.

To run the playbook, some prerequisites are required:

- A login node
- A controller node
- Compute nodes
- The controller node should be able to SSH into all nodes (using the same hostname as in `group_vars/all`)
- All nodes should have Ubuntu server installed

---

## Running Specific Roles or Tasks

Use the `--tags` option to run specific parts of the playbook:

```bash
ansible-playbook site.yml --tags "common,package,slurm"
```

---

## Development

- **Lint your playbooks** before committing:

```bash
ansible-lint site.yml
```

- If using **Homebrew**, ensure Ansible Galaxy collections are installed so `ansible-lint` can find them:

```bash
ansible-galaxy collection install -f community.general
ANSIBLE_COLLECTIONS_PATH=~/.ansible/collections ansible-lint site.yml
```
