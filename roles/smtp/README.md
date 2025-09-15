# SMTP

This role configures an **SMTP server** on the controller node to handle email notifications for SLURM jobs.
Carefully review the `group_vars/all.yml` file for the required variables such as `postfix_server_name`, `relay_host`, and mail credentials.

## Summary of Tasks

- Install Postfix and Mailutils along with required dependencies
- Configure Postfix for relay usage with authentication
- Add necessary configuration to `/etc/postfix/main.cf` and create SASL password file
- Generate Postfix lookup table
- Restart the Postfix service to apply changes

## Available Tags

- `smtp` - Set up and configure the SMTP server
