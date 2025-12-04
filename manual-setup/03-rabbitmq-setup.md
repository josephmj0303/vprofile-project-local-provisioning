Login to the RabbitMQ vm
 $ vagrant ssh rmq01
Verify Hosts  entry, if entries missing update it with IP and hostnames
 # cat /etc/hosts
Update OS with latest patches
 # apt update
 # apt upgrade
Install Dependencies
 Use rabbitmq script file to install rabbitmq server
Setup access to user test and make it admin
 # sh -c 'echo "[{rabbit, [{loopback_users, []}]}]." > /etc/rabbitmq/rabbitmq.config'
 # rabbitmqctl add_user test test
 # rabbitmqctl set_user_tags test administrator
 # systemctl restart rabbitmq-server
Starting the firewall and allowing the port 5672 to access rabbitmq
 # apt install firewalld -y
 # systemctl start firewalld
 # systemctl enable firewalld
 # ufw disable
 # firewall-cmd --state
 # firewall-cmd --add-port=5672/tcp
 # firewall-cmd --runtime-to-permanent
 # systemctl start rabbitmq-server
 # systemctl enable rabbitmq-server
 # systemctl status rabbitmq-server
