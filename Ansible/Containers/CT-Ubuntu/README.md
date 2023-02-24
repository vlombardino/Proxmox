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

Run playbook with the followin:
```bash
ansible-playbook -i 192.168.1.10, -u root -k ubuntu.proxmox.yml
```