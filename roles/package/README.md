# Packages

This role installs and configures software packages that can be loaded as environment modules on the cluster.
Some packages are also available as interactive apps via **Open OnDemand**.

The role supports installing multiple versions via variables and selective installation using Ansible tags.

## Supported Packages

- **Python**

  - Compiled from source
  - Environment modules with aliases (`python`, `pip`)
  - Registers kernels for JupyterLab

- **JupyterLab**

  - Installed in a virtual environment
  - Environment modules created
  - Available as an OOD app with example submission templates

- **R**

  - Compiled from source
  - Environment modules and `.Rprofile` configuration
  - Registers IRkernel for JupyterLab

- **RStudio Server**
  - Compiled from source
  - Environment modules created
  - Available as an OOD app with example submission templates

## Tags

- `package` - General package tasks
- `python` - Python tasks
- `jupyterlab` - JupyterLab tasks
- `jupyterlab-ood` - JupyterLab Open OnDemand tasks
- `r` - R tasks
- `rstudio` - RStudio Server tasks
- `rstudio-ood` - RStudio Server Open OnDemand tasks

## Notes

- Packages are installed under `/opt/apps/<package>/<version>` and environment modules under `/opt/modulefiles`.
- OOD app templates are copied from example repositories and configured.
