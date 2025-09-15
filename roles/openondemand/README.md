# Open OnDemand

This role sets up **Open OnDemand (OOD)** to provide a web-based interface for users to interact with the cluster.
It supports login and compute nodes with interactive desktop apps, branding, and additional configuration.

## Tasks

**Login node tasks:**

- Install Open OnDemand and dependencies (`ondemand`, `ondemand-dex`, `apache2`, SSL certs)
- Configure OOD portal and cluster settings
- Set up branding, custom CSS, logos, and dashboard environment variables
- Configure desktop submission forms and scripts
- Create initializers and custom settings (`ood.rb`) if scratch is enabled
- Restart required services (`apache2`, `ondemand-dex`)

**Compute node tasks:**

- Install dependencies for interactive apps (TurboVNC, xfce4, nmap, websockify)
- Configure desktop environment for interactive sessions
- Deploy scripts required for home directories and job submission

**Common tasks:**

- Generate and deploy SSL certificates (self-signed by default)
- Enable Apache SSL module and trust certificates
- Apply custom branding for login themes and dashboard

## Available Tags

- `ood` - General Open OnDemand tasks
- `ood-install` - Installation and core configuration
- `ood-branding` - Branding, logos, CSS, MOTD
- `ood-dependencies` - Install dependencies for interactive apps
- `ood-desktop` - Configure desktop environment and submission forms
- `ood-settings` - Additional settings, initializers, scratch integration
