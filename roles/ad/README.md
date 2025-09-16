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

## Notes

Some configuration is required on the Windows Server to get authentication working on all cluster nodes.\
On a new Windows Server setup, follow these steps:

1. Open the Windows Server Manager
2. Enable Remote Desktop in server properties (easier management)
3. Set up a static IP address
4. Change hostname
5. Restart server
6. If not all services start up, use the refresh button. (or manually start them)
7. Fix timezone if required
8. Install Windows Updates
9. Restart server
10. Via Windows Server Manager:
    - Manage
    - Add roles and features
    - Feature based
    - Select server
    - Active Directory domain services
    - Add features
    - Go through all settings (next)
    - Close
11. Click on the "!" and promote to AD domain controller
    - Add a new forest
    - Forest root domain: ad.example.com
    - Set highest level for functional level
    - Go through all settings (next)
12. Reboot
13. Check if server is domain controller member
    - Tools
    - Active directory users and computers
    - ad.example.com
    - Domain controller
    - Server should now be a member
14. Set up a reverse DNS lookup
    - DNS
    - Right click server entry - DNS Manager
    - Right click Reverse Lookup Zones
    - New zone - Primary zone - this domain - IPv4 - 192.168.0.x (or different range) - allow only secure dynamic

Don't forget to change the DNS of the controller (but preferably all nodes) to the windows server.\
This can be done in `/etc/systemd/resolved.conf`. Don't forget to restart service `systemd-resolved`.\
You can also change the Netplan (`/etc/netpla/50-cloud-init.yml`):

```
network: 
  version: 2 
  ethernets: 
    <eth port>:
      dhcp4: false
      addresses: 
        - <ip of node, not required> 
      routes:
        - to: default
          via: 192.168.0.1
      nameservers:
        addresses:
          - <static ip of windows server>
```

Afterwards, don't for get to run `netplan apply`
