Login to the Memcache vm
 $ vagrant ssh mc01
Verify Hosts entry, if entries missing update it with IP and hostnames
 # cat /etc/hosts
Update OS with latest patches
 # yum update -y
Install, start & enable memcache on port 11211
 # amazon-linux-extras install epel -y
 # yum install memcached -y
 # systemctl start memcached
 # systemctl enable memcached
 # systemctl status memcached
 # sed -i 's/127.0.0.1/0.0.0.0/g' /etc/sysconfig/memcached
 # systemctl restart memcached
Starting the firewall and allowing the port 11211 to access memcache
 # yum install firewalld -y
 # systemctl start firewalld
 # systemctl enable firewalld
 # firewall-cmd --add-port=11211/tcp
 # firewall-cmd --runtime-to-permanent
 # firewall-cmd --add-port=11111/udp
 # firewall-cmd --runtime-to-permanent
 # memcached -p 11211 -U 11111 -u memcached -d
