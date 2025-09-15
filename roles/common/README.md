# Common

This role sets up essential system-level dependencies and configurations that are required on all nodes.
It should always be executed **before any other roles** in the playbook.

## Summary of Tasks

- Update the system package manager cache (currently only supports Debian/Ubuntu)
- Install essential dependencies for Ansible (`python3-pexpect`)
- Clear the Message of the Day (MOTD) on login nodes

## Available Tags

- `common` - Run all common setup tasks
