# OpenLDAP

This role configures all nodes to **use OpenLDAP for authentication**, allowing users to log in with their LDAP accounts.
It sets up the LDAP server (on the controller) and ensures clients can authenticate securely using TLS/LDAPS.

## Summary of Tasks

**Controller node tasks:**

- Install and preconfigure `slapd` for LDAP server
- Generate self-signed TLS certificates for secure connections
- Configure LDAP server and schemas
- Configure `nsswitch.conf` and `nslcd` for LDAP authentication
- Install and configure **phpLDAPadmin** for LDAP management
- Enable PAM home directory creation for LDAP users

**Login and compute node tasks:**

- Install LDAP client dependencies
- Configure `nsswitch.conf` and `nslcd` to connect to the LDAP server
- Ensure PAM creates home directories for LDAP users

## Available Tags

- `ldap` - Run all LDAP-related tasks
- `ldap-controller` - Tasks specific to the LDAP server/controller node
- `ldap-node` - Tasks specific to login/compute nodes
- `phpldapadmin` - Configure phpLDAPadmin web interface
