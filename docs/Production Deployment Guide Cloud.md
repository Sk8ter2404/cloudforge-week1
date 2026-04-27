Production Deployment Guide: Cloud Infrastructure

**Role:** Infrastructure Lead  
**Document Version:** 1.0  
**Status:** Production Ready  

## 1. Architecture Overview
This document outlines the standard operating procedure for deploying our high-availability (HA) VPS hosting infrastructure. The environment utilizes Proxmox VE as the base hypervisor, FOSSBilling for customer management, and HAProxy for load balancing, all secured behind strict firewall rules.

## 2. Hardware Allocation

### Proxmox Host (testhost)
- **CPU:** 12 Cores
- **RAM:** 46.89 GB
- **Storage:** 94 GB

### Virtual Machine Inventory
| VM ID | Hostname | Role | CPU | RAM | Storage |
|-------|----------|------|-----|-----|---------|
| 100 | `fossbilling` | Customer Portal & Database | 4 vCPU | 8 GB RAM | 20 GB |
| 200 | `haproxy-lb` | Incoming Traffic Router | 2 vCPU | 2 GB RAM | 12 GB |
| [ID] | `monitor-01` | Grafana / Prometheus Metrics | 2 vCPU | 4 GB RAM | 20 GB |

## 3. Network & Security Configuration

### Network Segmentation
The infrastructure utilizes a dual-network approach to separate public management traffic from internal application traffic:
* **Public Management:** Attached to `vmbr0` (Allows external access to Proxmox GUI).
* **Private Network (`localnetwork`):** Attached to `vmbr1` (Handles all internal communication between VMs without exposing them to the internet).

### Firewall (UFW) Rules
The following strict `ufw` rules are applied to the core FOSSBilling VM to ensure minimal attack surface:

```bash
# Default deny incoming, allow outgoing
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow SSH for administration
sudo ufw allow 22/tcp

# Allow Web Traffic (HTTP/HTTPS)
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Enable Firewall
sudo ufw enable


4. Step-by-Step Deployment Checklist
Phase 1: Base Hypervisor Setup
Install Proxmox VE on bare-metal hardware.

Configure static IP address on vmbr0.

Create localnetwork bridge (vmbr1) for internal VM traffic.

Update Proxmox repositories and install latest security patches.

Phase 2: Virtual Machine Provisioning
Upload Ubuntu 24.04 ISO to Proxmox local storage.

Provision VM 100 (FOSSBilling) and VM 200 (HAProxy) according to the Hardware Allocation table.

Assign both VMs to the localnetwork interface.

Install base OS and configure SSH key-based authentication (disable root password login).

Phase 3: Application Deployment
Database: Install MariaDB on VM 100, secure installation, and create fossbilling database and user.

Web Server: Install Apache2/Nginx and PHP 8.1+ dependencies.

Application: Extract FOSSBilling release to /var/www/fossbilling.

Permissions: Set ownership to www-data (chown -R www-data:www-data /var/www/fossbilling).

Configuration: Copy config-sample.php to config.php and inject database credentials.

Phase 4: High Availability & Scaling Setup
Add Proxmox node to the datacenter cluster via pvecm.

Configure shared network storage (NFS/Ceph) for VM disks.

Enable HA for VM 100 in the Proxmox Datacenter GUI to ensure automatic failover.

Deploy the Python Load Balancer script (load_balancer.py) and configure Proxmox API token access.