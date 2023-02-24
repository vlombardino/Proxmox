# [Proxmox module](https://docs.ansible.com/ansible/latest/collections/community/general/proxmox_module.html)
> Create a CT in Proxmox using Ansible

## Configure Proxmox module
Check if module is installed
```bash
ansible-galaxy collection list
```

Install with the following:
```bash
ansible-galaxy collection install community.general
```

File list:
- [ansible.cfg](https://raw.githubusercontent.com/vlombardino/Proxmox/master/Ansible/Containers/CT-Ubuntu/ansible.cfg): Ansible configuration settings
- [install-dep.yml](https://raw.githubusercontent.com/vlombardino/Proxmox/master/Ansible/Containers/CT-Ubuntu/install-dep.yml): Ansible playbook to install required depencies on Proxmox server
- [remove-dep.yml](https://raw.githubusercontent.com/vlombardino/Proxmox/master/Ansible/Containers/CT-Ubuntu/remove-dep.yml): Ansible playbook to remove required dependencies on Proxmox server
- [ubuntu-proxmox.yml](https://raw.githubusercontent.com/vlombardino/Proxmox/master/Ansible/Containers/CT-Ubuntu/ubuntu.proxmox.yml): Ansible playbook to install container on Proxmox server running ubuntu 22.04-1

Run playbook with the followin:
```bash
ansible-playbook -i 192.168.1.10, -u root -k ubuntu.proxmox.yml
```