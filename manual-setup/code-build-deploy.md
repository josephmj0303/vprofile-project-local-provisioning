Download Source code
 # git clone -b main https://github.com/hkhcoder/vprofile-project.git
Update configuration
 # cd vprofile-project
 # vim src/main/resources/application.properties
 # Update file with backend server details
Build code
 Run below command inside the repository (vprofile-project)
 # mvn install
Deploy artifact
 # systemctl stop tomcat
 # rm -rf /usr/local/tomcat/webapps/ROOT*
 # cp target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war
 # systemctl start tomcat
 # chown tomcat.tomcat /usr/local/tomcat/webapps -R
 # systemctl restart tomcat
