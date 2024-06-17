**VPROFILE PROJECT SETUP![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.001.png)**

**Prerequisite**

1. Oracle VM Virtualbox
1. Vagrant
1. Vagrant plugins

   Execute below command in your computer to install hostmanager plugin

$ vagrant plugin install vagrant-hostmanager![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.002.png)

4. Git bash or equivalent editor

**VM SETUP**

1. Clone source code.
1. Cd into the repository.
1. Switch to the main branch.
1. cd into vagrant/Manual_provisioning

Bring up vm’s

_$ vagrant up![ref1]_

NOTE: Bringing up all the vm’s may take a long time based on various factors. If vm setup stops in the middle run “vagrant up” command again.

INFO: All the vm’s hostname and /etc/hosts file entries will be automatically updated.

![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.004.png)

**PROVISIONING**

**Services**

1. Nginx => Web Service![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.005.png)
1. Tomcat => Application Server
1. RabbitMQ => Broker/Queuing Agent
1. Memcache => DB Caching
1. ElasticSearch => Indexing/Search service
1. MySQL => SQL Database

Setup should be done in below mentioned order

**MySQL (Database SVC) Memcache (![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.006.png)DB Caching SVC) RabbitMQ (Broker/Queue SVC) Tomcat (Application SVC) Nginx (Web SVC)**

**1. MYSQL Setup**

Login to the db vm

_$ vagrant ssh db01![ref2]_

Verify Hosts entry, if entries missing update the it with IP and hostnames

- _cat /etc/hosts![ref2]_

Update OS with latest patches

- _yum update -y![ref2]_

Set Repository

- _yum install epel-release -y![ref2]_

Install Maria DB Package

- _yum install git mariadb-server -y![ref2]_

Starting & enabling mariadb-server

- _systemctl start mariadb![ref3]_
- _systemctl enable mariadb_

RUN mysql secure installation script.

- _mysql_secure_installation![ref4]_

**\*NOTE**: Set db root password, I will be using **admin123 as password\***

Set root password? [Y/n] **Y ![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.010.png)**New password:

Re-enter new password: Password updated successfully! Reloading privilege tables..

... Success!

By default, a MariaDB installation has an anonymous user, allowing anyone to log into MariaDB without having to have a user account created for them. This is intended only for testing, and to make the installation go a bit smoother. You should remove them before moving into a production environment.

Remove anonymous users? [Y/n] **Y**

... Success!

Normally, root should only be allowed to connect from 'localhost'. This ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] **n**

... skipping.

By default, MariaDB comes with a database named 'test' that anyone can access. This is also intended only for testing, and should be removed before moving into a production environment.

Remove test database and access to it? [Y/n] **Y**

- Dropping test database...

... Success!

- Removing privileges on test database... ... Success!

Reloading the privilege tables will ensure that all changes made so far will take effect immediately.

Reload privilege tables now? [Y/n] **Y**

... Success!

Set DB name and users.

- _mysql -u root -padmin123![ref4]_

_mysql> create database accounts;![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.011.png)_

_mysql> grant all privileges on accounts.\* TO 'admin'@'%' identified by 'admin123'; mysql> FLUSH PRIVILEGES;_

_mysql> exit;_

Download Source code & Initialize Database.

- _git clone -b main https://github.com/hkhcoder/vprofile-project.git![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.012.png)_
- _cd vprofile-project_
- _mysql -u root -padmin123 accounts < src/main/resources/db_backup.sql_
- _mysql -u root -padmin123 accounts_

_mysql> show tables; mysql>![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.013.png) exit;_

Restart mariadb-server

- _systemctl restart mariadb![ref4]_

Starting the firewall and allowing the mariadb to access from port no. 3306

- systemctl start firewalld![ref5]
- systemctl enable firewalld
- firewall-cmd --get-active-zones
- firewall-cmd --zone=public --add-port=3306/tcp --permanent
- firewall-cmd --reload
- systemctl restart mariadb

**2.MEMCACHE SETUP**

Login to the Memcache vm

_$ vagrant ssh mc01![ref2]_

Verify Hosts entry, if entries missing update the it with IP and hostnames

- _cat /etc/hosts![ref2]_

Update OS with latest patches

- _yum update -y![ref2]_

Install, start & enable memcache on port 11211

- _sudo dnf install epel-release -y![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.015.png)_
- _sudo dnf install memcached -y_
- _sudo systemctl start memcached_
- _sudo systemctl enable memcached_
- _sudo systemctl status memcached_
- _sed -i 's/127.0.0.1/0.0.0.0/g' /etc/sysconfig/memcached_
- _sudo systemctl restart memcached_

Starting the firewall and allowing the port 11211 to access memcache

- _firewall-cmd --add-port=11211/tcp![ref6]_
- _firewall-cmd --runtime-to-permanent_
- _firewall-cmd --add-port=11111/udp_
- _firewall-cmd --runtime-to-permanent_
- _sudo memcached -p 11211 -U 11111 -u memcached -d_

**3.RABBITMQ SETUP**

Login to the RabbitMQ vm

_$ vagrant ssh rmq01![ref4]_

Verify Hosts entry, if entries missing update the it with IP and hostnames

- _cat /etc/hosts![ref4]_

Update OS with latest patches

- _yum update -y![ref4]_

Set EPEL Repository

- _yum install epel-release -y![ref4]_

Install Dependencies

- _sudo yum install wget -y![ref6]_
- _cd /tmp/_
- _dnf -y install centos-release-rabbitmq-38_
- _dnf --enablerepo=centos-rabbitmq-38 -y install rabbitmq-server_
- _systemctl enable --now rabbitmq-server_

Setup access to user test and make it admin

- _sudo sh -c 'echo "[{rabbit, [{loopback_users, []}]}]." > /etc/rabbitmq/rabbitmq.config'![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.017.png)_
- _sudo rabbitmqctl add_user test test_
- _sudo rabbitmqctl set_user_tags test administrator_
- _sudo systemctl restart rabbitmq-server![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.018.png)_

Starting the firewall and allowing the port 5672 to access rabbitmq

- _firewall-cmd --add-port=5672/tcp![ref5]_
- _firewall-cmd --runtime-to-permanent_
- _sudo systemctl start rabbitmq-server_
- _sudo systemctl enable rabbitmq-server_
- _sudo systemctl status rabbitmq-server_

**4.TOMCAT SETUP**

Login to the tomcat vm _$ vagrant ssh app01![ref4]_

Verify Hosts entry, if entries missing update the it with IP and hostnames

- _cat /etc/hosts![ref4]_

Update OS with latest patches

- _yum update -y![ref4]_

Set Repository

- _yum install epel-release -y![ref4]_

Install Dependencies

- dnf -y install java-11-openjdk java-11-openjdk-devel![ref2]
- dnf install git maven wget -y![ref4]

Change dir to /tmp

- _cd /tmp/![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.019.png)_

Download & Tomcat Package

- _wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.75/bin/apache-tomcat-9.0.75.tar.gz![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.020.png)_
  - _tar xzvf apache-tomcat-9.0.75.tar.gz![ref2]_

Add tomcat user

- _useradd --home-dir /usr/local/tomcat --shell /sbin/nologin tomcat![ref2]_

Copy data to tomcat home dir

- _cp -r /tmp/apache-tomcat-9.0.75/\* /usr/local/tomcat/![ref2]_

Make tomcat user owner of tomcat home dir

- _chown -R tomcat.tomcat /usr/local/tomcat![ref2]_

Setup systemctl command for tomcat

Create tomcat service file

- _vi /etc/systemd/system/tomcat.service![ref4]_

Update the file with below content

[Unit] ![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.021.png)Description=Tomcat After=network.target

[Service]

User=tomcat

WorkingDirectory=/usr/local/tomcat Environment=JRE_HOME=/usr/lib/jvm/jre Environment=JAVA_HOME=/usr/lib/jvm/jre Environment=CATALINA_HOME=/usr/local/tomcat Environment=CATALINE_BASE=/usr/local/tomcat ExecStart=/usr/local/tomcat/bin/catalina.sh run ExecStop=/usr/local/tomcat/bin/shutdown.sh SyslogIdentifier=tomcat-%i

[Install] ![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.022.png)WantedBy=multi-user.target

Reload systemd files

- _systemctl daemon-reload![ref7]_

Start & Enable service

- _systemctl start tomcat![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.024.png)_
- _systemctl enable tomcat_

Enabling the firewall and allowing port 8080 to access the tomcat

- _systemctl start firewalld![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.025.png)_
- _systemctl enable firewalld_
- _firewall-cmd --get-active-zones_
- _firewall-cmd --zone=public --add-port=8080/tcp --permanent_
- _firewall-cmd --reload_

**CODE BUILD & DEPLOY (app01)**

Download Source code

- _git clone -b main https://github.com/hkhcoder/vprofile-project.git![ref1]_

Update configuration

- cd vprofile-project![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.026.png)
- vim src/main/resources/application.properties
- Update file with backend server details

**Build code**

_Run below command inside the repository (vprofile-project)_

- _mvn install![ref7]_

Deploy artifact

- _systemctl stop tomcat![ref7]_
- _rm -rf /usr/local/tomcat/webapps/ROOT\*![ref6]_
- _cp target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war_
- _systemctl start tomcat_
- _chown tomcat.tomcat /usr/local/tomcat/webapps -R_
- _systemctl restart tomcat_

**5.NGINX SETUP**

Login to the Nginx vm

_$ vagrant ssh web01 ![ref3]$ sudo -i_

Verify Hosts entry, if entries missing update the it with IP and hostnames

- _cat /etc/hosts![ref2]_

Update OS with latest patches

- _apt update![ref3]_
- _apt upgrade_

Install nginx

- _apt install nginx -y![ref2]_

Create Nginx conf file

- vi /etc/nginx/sites-available/vproapp![ref2]

Update with below content

_upstream vproapp {![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.027.png)_

_server app01:8080;_

_}_

_server {_

_listen 80;_

_location / {_

_proxy_pass http://vproapp;_

_} }![](Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.028.png)_

Remove default nginx conf

- _rm -rf /etc/nginx/sites-enabled/default![ref2]_

Create link to activate website

- _ln -s /etc/nginx/sites-available/vproapp /etc/nginx/sites-enabled/vproapp![ref2]_

Restart Nginx

- _systemctl restart nginx![ref2]_

[ref1]: Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.003.png
[ref2]: Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.007.png
[ref3]: Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.008.png
[ref4]: Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.009.png
[ref5]: Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.014.png
[ref6]: Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.016.png
[ref7]: Aspose.Words.727762e1-087c-4faf-8e5f-48ca293c97cf.023.png
