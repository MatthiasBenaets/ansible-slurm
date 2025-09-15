# Active Directory

This role prepares all nodes to **join an Active Directory (AD) domain**, allowing users to log in with their AD accounts.
It configures the necessary authentication services and ensures proper PAM integration for home directories.

## Summary of Tasks

- Install and configure **Realmd** for domain joining
- Configure **Kerberos** (`krb5.conf`) for authentication
- Join nodes to the Active Directory domain
- Install and configure **SSSD** for user authentication
- Update **PAM** to automatically create home directories for AD users

## Available Tags

- `ad` - Run all AD-related tasks
- `realmd` - Configure Realmd and join AD domain
- `sssd` - Configure SSSD and PAM for AD users
