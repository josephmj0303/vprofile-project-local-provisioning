Login to the Nginx vm
 $ vagrant ssh web01
 $ sudo-i
Verify Hosts entry, if entries missing update it with IP and hostnames
 # cat /etc/hosts
Update OS with latest patches
 # apt update
 # apt upgrade
Install nginx
 # apt install nginx -y
Create Nginx conf file
 # vi /etc/nginx/sites-available/vproapp
Update with below content
 upstream vproapp {
 server app01:8080;
 }
 server {
 listen 80;
 location / {
 proxy_pass http://vproapp;
 }
 }
Remove default nginx conf
 # rm -rf /etc/nginx/sites-enabled/default
Create link to activate website
 # ln -s /etc/nginx/sites-available/vproapp /etc/nginx/sites-enabled/vproapp
Restart Nginx
 # systemctl restart nginx
