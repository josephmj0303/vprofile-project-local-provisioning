# Port Mapping Overview

This document summarizes all ports used by the VProfile multi-tier environment.  
These ports are required for service communication, application access, and internal VM-to-VM interactions.

---

## 1. Application & Web Tier

| Component | Port | Description |
|----------|------|-------------|
| Nginx | **80** | Public entry point for the VProfile application |
| Tomcat | **8080** | Application server hosting the WAR file |

---

## 2. Backend Services

| Service | Port | Description |
|---------|------|-------------|
| MySQL / MariaDB | **3306** | Database communication |
| Memcached | **11211** | Distributed caching |
| RabbitMQ (AMQP) | **5672** | Application message queue communication |
---

## 3. Host / Vagrant Access

| Purpose | Port | Description |
|---------|------|-------------|
| SSH (optional) | **22** | Vagrant/VM SSH access (default per VM) |

---

## 4. Notes

- All ports are accessible only within the **private host-only network**.
- Only Nginx (port 80) is intended to be externally accessed via the host machine.
- When required, forwarding can be configured in the Vagrantfile using:

config.vm.network "forwarded_port", guest: <port>, host: <port>
```
Port usage is aligned with the provisioning sequence and internal architecture.
```
