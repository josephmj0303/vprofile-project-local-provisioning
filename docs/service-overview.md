# Service Overview

This document summarizes all major services in the VProfile application architecture, their purpose, and how they interact within the multi-tier setup.

---

## 1. Web Tier: Nginx

- Acts as the reverse proxy between users and the application server
- Terminates HTTP requests
- Forwards traffic to Tomcat running on app01
- Helps improve performance, security, and URL handling

---

## 2. Application Tier: Tomcat 9

- Runs the VProfile Java web application (WAR file)
- Handles business logic and all server-side processing
- Communicates with MySQL, Memcached, and RabbitMQ
- Hosted on app01

---

## 3. Database Tier: MySQL / MariaDB

- Stores all persistent application data
- Used for:
  - User authentication
  - Application transactions
  - Persistent configuration
- Hosted on db01

---

## 4. Cache Tier: Memcached

- Provides distributed caching for frequently accessed data
- Reduces load on the application server
- Improves application performance
- Hosted on mc01

---

## 5. Messaging Tier: RabbitMQ

- Handles asynchronous operations
- Supports background workers in the VProfile app
- Provides AMQP protocol + Admin UI
- Hosted on rmq01

---

## 6. Inter-Service Communication

| Source | Destination | Purpose |
|--------|-------------|---------|
| web01 (Nginx) | app01 (Tomcat) | Reverse proxy request handling |
| app01 | db01 | Database operations |
| app01 | mc01 | Cache read/write |
| app01 | rmq01 | Message publishing/consuming |

---

## 7. Service Dependencies (Top-Down)

1. MySQL  
2. Memcached  
3. RabbitMQ  
4. Tomcat  
5. Nginx  

This ensures Tomcat starts only after its backend services are ready.

---

## 8. Notes

- All services run in isolated VMs for clean debugging.
- The architecture reflects real-world enterprise deployment patterns.
- Each service is installed and configured through its respective Bash provisioning script.
