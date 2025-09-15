# Quota

This role configures **disk quotas on the home directory** to help manage storage usage.
Users will be notified when approaching or exceeding their quota limits.
A soft and hard limit can be configured. Quotas help maintain a clean home directory and encourage using the scratch directory for large or temporary files.

> Note: A separate partition for `/home` is required for quotas to work properly.

## Summary of Tasks

**Controller node tasks:**

- Install quota management packages (`quota`, `quotatool`)
- Enable user and group quotas on `/home` in `/etc/fstab` and remount
- Initialize quota files and enable quota enforcement
- Set up `rpc.rquotad` service for quota reporting
- Create scripts for setting quotas and nightly enforcement
- Configure PAM to call the quota script for interactive and non-interactive sessions
- Set grace periods for quota enforcement

**Login node tasks:**

- Display current quota usage in the Message of the Day (MOTD)

## Available Tags

- `quota` - Run all quota configuration tasks
- `quota-motd` - Configure MOTD message to show quota usage
