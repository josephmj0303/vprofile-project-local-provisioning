# VM Details

This document describes the virtual machines created through the Vagrantfile and the purpose of each VM within the VProfile architecture.

---

## 1. Virtual Machine Summary

| VM Name | Hostname | Role | Service | Recommended Resources |
|--------|-----------|------|---------|------------------------|
| **db01** | db01.vprofile.local | Database VM | MySQL/MariaDB | 1 CPU, 600 MB RAM |
| **mc01** | mc01.vprofile.local | Cache VM | Memcached | 1 CPU, 600 MB RAM |
| **rmq01** | rmq01.vprofile.local | Messaging VM | RabbitMQ | 1 CPU, 600 MB RAM |
| **app01** | app01.vprofile.local | Application VM | Tomcat 9 + VProfile WAR | 1 CPU, 800 MB RAM |
| **web01** | web01.vprofile.local | Web VM | Nginx Reverse Proxy | 1 CPU, 800 MB RAM |

Actual CPU and RAM values should match your Vagrantfile settings.

---

## 2. Networking Details

All VMs operate inside a **host-only private network** provisioned by Vagrant.

| VM Name | Typical IP | Notes |
|---------|------------|-------|
| db01 | 192.168.56.15 | Used by app01 to connect to MySQL |
| mc01 | 192.168.56.14 | Used for Memcached operations |
| rmq01 | 192.168.56.13 | AMQP + Web UI |
| app01 | 192.168.56.12 | Hosts Tomcat |
| web01 | 192.168.56.11 | Public entry point to the application |

(Use your actual IP allocation if different.)

---

## 3. Provisioning Method

All VMs are automatically configured using shell scripts located under:

automated-setup/provisioning/

Each VM has its own script:

- `mysql.sh`
- `memcache.sh`
- `rabbitmq.sh`
- `tomcat.sh`
- `nginx.sh`
- `backend.sh` (if used)

---

## 4. Purpose of Each VM

### **db01 (Database VM)**
Stores persistent application data and provides the relational database layer for the VProfile application.

### **mc01 (Cache VM)**
Hosts Memcached, enabling faster page loads and efficient session handling.

### **rmq01 (Message Broker VM)**
Runs RabbitMQ and supports asynchronous operations triggered by the application.

### **app01 (Application Server VM)**
Hosts Tomcat 9 and runs the compiled Java WAR file of the VProfile application.

### **web01 (Web Layer VM)**
Runs Nginx and acts as the reverse proxy entry point for end-users.

---

## 5. Notes

- All VMs are Linux-based (CentOS/Ubuntu depending on your setup).
- Vagrant provisioning is idempotent—running scripts multiple times is safe.
- VM names, hostnames, and IPs are consistent across manual and automated setups.
