Troubleshooting Guide

This document covers common issues encountered during the manual setup of the VProfile multi-tier environment. Each section includes symptoms, possible causes, and recommended fixes based on the provisioning steps in this project.

1. MySQL / MariaDB Issues
Issue: MySQL service failing to start

Symptoms:

systemctl status mariadb shows failed state

Errors related to permissions or missing data directory

Possible Causes:

Incorrect ownership of /var/lib/mysql

Corrupted or uninitialized data directory

Fix:

sudo chown -R mysql:mysql /var/lib/mysql
sudo systemctl restart mariadb


If the DB was not initialized:

sudo mysql_install_db
sudo systemctl restart mariadb

Issue: Unable to log in to MySQL

Symptoms:

Access denied for user 'root'@'localhost'

Wrong root password

Fix: Reset root authentication plugin

sudo mysql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'password';
FLUSH PRIVILEGES;

2. Memcached Issues
Issue: Memcached fails to start

Symptoms:

systemctl status memcached shows error

Port 11211 already in use

Fix:
Check if another instance is running:

sudo ss -ltnp | grep 11211


Restart:

sudo systemctl restart memcached

3. RabbitMQ Issues
Issue: RabbitMQ server not starting

Symptoms:

Service fails with a BEAM/Erlang error

Logs show "node name mismatch"

Fix:
Ensure hostname matches:

sudo rabbitmqctl status
hostname -f


If mismatch:

sudo rabbitmqctl reset
sudo systemctl restart rabbitmq-server

Issue: RabbitMQ management UI not loading

Symptoms:

http://<rmq01-ip>:15672 unreachable

Fix: Enable plugin manually

sudo rabbitmq-plugins enable rabbitmq_management
sudo systemctl restart rabbitmq-server

4. Tomcat Issues
Issue: Tomcat not starting

Symptoms:

systemctl status tomcat fails

Java-related error

Fix:
Check Java installation:

java -version


Reinstall OpenJDK if missing:

sudo yum install -y java-11-openjdk

Issue: Application not loading in Tomcat

Symptoms:

WAR file not deployed

404 Not Found when accessing /vprofile/

Fix:

Ensure WAR file is placed in:
/usr/share/tomcat/webapps/

Check Tomcat logs:

sudo tail -f /var/log/tomcat/catalina.out

5. Nginx Issues
Issue: Nginx returns 502 Bad Gateway

Symptoms:

Nginx is running but cannot reach Tomcat

Possible Causes:

Incorrect upstream IP/port

Tomcat not running

Fix:
Check Nginx configuration:

sudo cat /etc/nginx/conf.d/vprofile.conf


Validate Tomcat service:

systemctl status tomcat


Reload Nginx:

sudo systemctl restart nginx

Issue: Nginx configuration syntax error

Symptoms:

Restart fails

Output: nginx: [emerg] invalid...

Fix:
Test config before restarting:

sudo nginx -t


Fix reported syntax errors and then:

sudo systemctl restart nginx

6. Network & Connectivity Issues
Issue: VMs not reachable over private IP

Causes:

Wrong IP in Vagrantfile

Interface not configured

Fix:
Restart networking:

sudo systemctl restart network


Reboot VM:

sudo reboot

Issue: Services cannot resolve hostnames

Fix:
Check /etc/hosts entries:

sudo cat /etc/hosts


Ensure entries like:

192.168.56.11 db01
192.168.56.12 mc01
192.168.56.13 rmq01
192.168.56.14 app01
192.168.56.15 web01

7. Command Not Found / Package Installation Issues
Issue: Package not found during yum install

Fix:
Enable EPEL repository:

sudo yum install -y epel-release
sudo yum update -y


Clean YUM cache:

sudo yum clean all

8. General Debugging Commands
Check service status:
systemctl status <service-name>

View last 100 log lines:
sudo journalctl -u <service-name> -n 100

Check open ports:
sudo ss -ltnp

Check if hostname is correct:
hostname
hostname -f

Verify connectivity:
ping <ip-address>
curl http://<ip>:<port>

9. Suggested Workflow for Debugging

Check the service
systemctl status <service>

Check logs
journalctl -u <service> -n 100

Check network reachability
ping, curl, ss

Check configuration files
/etc/<service>/

Re-run provisioning

vagrant provision <vm-name>


Reboot VM if necessary
sudo reboot

10. Notes

Troubleshooting steps mirror the issues commonly found in the PDF manual setup.

Most errors in multi-tier Vagrant environments occur due to IP mismatch, hostname issues, missing packages, or service dependency failures.

Re-running provisioning scripts is safe and recommended when configuration changes are made.
