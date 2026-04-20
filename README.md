# ansible-jupyterhub

Ansible playbook for JupyterHub deployment on Ubuntu 24.04.

Installs JupyterHub 4.x with JupyterLab interface, idle culler,
and optional R kernel (shared with RStudio Server conda environment).

## Features

- **Conda-based** — JupyterHub and JupyterLab installed via Miniforge, reproducible and upgradable
- **JupyterLab by default** — modern interface with file browser, terminal, multi-tabs
- **Idle culler** — automatically shuts down inactive user servers
- **R kernel** — optional, reuses the RStudio Server conda environment (no duplicate R install)
- **PAM authentication** — users log in with their Linux credentials
- **User conda kernels** — users can register their own conda environments as Jupyter kernels

## Usage

```bash
# ARTbio production (behind nginx at /jupyter/)
ansible-playbook -i inventory/artbio/hosts.yml playbook.yml

# GCE test VM
ansible-playbook -i inventory/gce-test/hosts.yml playbook.yml
```

## User kernel setup

Users can create their own conda environments and register them as Jupyter kernels:

```bash
conda create -n myenv python=3.12
conda activate myenv
conda install ipykernel
python -m ipykernel install --user --name=myenv --display-name="My Environment"
```

The kernel will appear in JupyterLab's launcher after restarting the server.

## Inventories

- `inventory/artbio/` — ARTbio production server (behind nginx, R kernel enabled)
- `inventory/gce-test/` — GCE test VM (behind nginx, minimal)
