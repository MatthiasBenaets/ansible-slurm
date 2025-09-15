# Timedate

This role ensures that all nodes have the correct **time, date, and timezone**.
It should be executed **early in the playbook**, as consistent time across nodes is crucial for a SLURM cluster.

## Summary of Tasks

- Set the system timezone according to the configured variable (`timezone`)

## Available Tags

- `timedate` - Apply timezone configuration
