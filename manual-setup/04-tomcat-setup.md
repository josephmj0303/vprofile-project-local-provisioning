Login to the tomcat vm
 $ vagrant ssh app01
Verify Hosts entry, if entries missing update it with IP and hostnames
 # cat /etc/hosts
Update OS with latest patches
 # yum update -y
Set Repository
 # amazon-linux-extras install epel -y
Install Dependencies
 # amazon-linux-extras install java-openjdk11 
 # sudo yum install java-11-openjdk-devel
 # yum install git maven wget -y
Change dir to /tmp
 # cd /tmp/
Download & Tomcat Package
 # wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.75/bin/apache-tomcat-9.0.75.tar.gz
 # tar xzvf apache-tomcat-9.0.75.tar.gz
Add tomcat user
 # useradd --home-dir /usr/local/tomcat --shell /sbin/nologin tomcat
Copy data to tomcat home dir
 # cp -r /tmp/apache-tomcat-9.0.75/* /usr/local/tomcat/
Make tomcat user owner of tomcat home dir
 # chown -R tomcat.tomcat /usr/local/tomcat
Setup systemctl command for tomcat
 Create tomcat service file
 # vi /etc/systemd/system/tomcat.service
 Update the filewith below content
 [Unit]
 Description=Tomcat
 After=network.target
 [Service]
 User=tomcat
 WorkingDirectory=/usr/local/tomcat
 Environment=JRE_HOME=/usr/lib/jvm/jre
 Environment=JAVA_HOME=/usr/lib/jvm/jre
 Environment=CATALINA_HOME=/usr/local/tomcat
 Environment=CATALINE_BASE=/usr/local/tomcat
 ExecStart=/usr/local/tomcat/bin/catalina.sh run
 ExecStop=/usr/local/tomcat/bin/shutdown.sh
 SyslogIdentifier=tomcat-%i
 [Install]
 WantedBy=multi-user.target
Reload systemd files
 # systemctl daemon-reload
Start & Enable service
 # systemctl start tomcat
 # systemctl enable tomcat
Enabling the firewall and allowing port 8080 to access the tomcat
 # yum install firewalld -y
 # systemctl start firewalld
 # systemctl enable firewalld
 # firewall-cmd --get-active-zones
 # firewall-cmd --zone=public --add-port=8080/tcp --permanent
 # firewall-cmd --reload
