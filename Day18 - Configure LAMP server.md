# Day 18: Configure LAMP server

## Question

xFusionCorp Industries is planning to host a `WordPress` website on their infra in `Stratos Datacenter`. They have already done infrastructure configuration - for example, on the storage server they already have a shared directory `/vaw/www/html` that is mounted on each app host under `/var/www/html` directory. Please perform the following steps to accomplish the task:

**Task:**  

a. Install httpd, php and its dependencies on all app hosts.

b. Apache should serve on port `5002 ` within the apps.

c. Install/Configure `MariaDB server` on DB Server.

d. Create a database named `kodekloud_db8` and create a database user named `kodekloud_tim` identified as password `Rc5C9EyvbU`. Further make sure this newly created user is able to perform all operation on the database you created.

e. Finally you should be able to access the website on LBR link, by clicking on the `App` button on the top bar. You should see a message like `App is able to connect to the database using user kodekloud_tim`

---

## Solution

1. **On All App Servers (Install Apache + PHP)**

```bash
# Login on App Servers
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

# Update packages
sudo yum install -y httpd php php-mysqlnd php-cli php-gd php-curl php-xml

# Change Apache port to 5002
sudo sed -i 's/^Listen 80/Listen 5002/' /etc/httpd/conf/httpd.conf

# Allow port in firewall (if running)
sudo firewall-cmd --permanent --add-port=5002/tcp
sudo firewall-cmd --reload

# Enable and start Apache
sudo systemctl enable httpd
sudo systemctl start httpd
```

---

2. **On DB Server (Install MariaDB)**

```bash
# Login on DB Server
ssh natasha@stdb01

# Install MariaDB server
sudo yum install -y mariadb-server

# Enable and start MariaDB
sudo systemctl enable mariadb
sudo systemctl start mariadb
```

---

3. **Configure Database**

```bash
# Login to MariaDB
mysql -u root
# or
sudo mysql

# Create DB
CREATE DATABASE kodekloud_db8;

# Create user and grant privileges
CREATE USER 'kodekloud_tim'@'%' IDENTIFIED BY 'Rc5C9EyvbU';
GRANT ALL PRIVILEGES ON kodekloud_db8.* TO 'kodekloud_tim'@'%';
FLUSH PRIVILEGES;
EXIT;
```

✔️ `%` ensures user can connect from any app server.

---

4. **Test Access**

Go to the LBR App URL (provided in KodeKloud lab).

You should see:

```pgsql
App is able to connect to the database using user kodekloud_tim
```

