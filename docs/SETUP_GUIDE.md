# Setup Guide - Week 1

ZFS Storage Pool: MainStorage
Network Bridge: vmbr0 (192.168.0.230/24)

Ubuntu Template (9000):
- Cloud image downloaded and imported to MainStorage
- Configured with cloud-init and QEMU agent

Automation Script:
- Location: /opt/cloud-automation/create_vm.py
- Uses Proxmox API token "root@pam!automation"
