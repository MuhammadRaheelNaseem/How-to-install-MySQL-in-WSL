## How to Install MySQL on WSL 2 (Ubuntu)

---

### Table of Contents

- [Update WSL 2 Ubuntu](#update-wsl-2-ubuntu)
- [Install MySQL Server](#install-mysql-server)
- [Confirm Installation](#confirm-installation)
- [Secure the MySQL Server](#secure-the-mysql-server)
- [Verify MySQL is Running](#verify-mysql-is-running)
- [Allow Remote Access](#allow-remote-access)
- [Configure HeidiSQL](#configure-heidisql)
- [Change the Default Port](#change-the-default-port)
- [Start MySQL Automatically When WSL Starts](#start-mysql-automatically-when-wsl-starts)
- [Manually Start MySQL](#manually-start-mysql)
- [Troubleshooting](#troubleshooting)
  - [Typical Errors and Possible Reasons](#typical-errors-and-possible-reasons)
  - [Basic Troubleshooting](#basic-troubleshooting)
  - [Check the Server is Running](#check-the-server-is-running)
  - [Check the MySQL Server for Errors](#check-the-mysql-server-for-errors)
  - [Check the Port](#check-the-port)
- [Stop MySQL and WSL](#stop-mysql-and-wsl)

---

### Update WSL 2 Ubuntu

Before starting the installation, make sure your WSL 2 is up-to-date. Run the following commands in your WSL terminal:

```bash
sudo apt update && sudo apt upgrade
```

Enter your password when prompted and wait for the packages to install.

---

### Install MySQL Server

Once the packages are updated, you can install MySQL server with the following command:

```bash
sudo apt install mysql-server
```

Wait for the server to install. You'll see a lot of information on the screen.

---

### Confirm Installation

To confirm the installation, check the MySQL version:

```bash
mysql --version
```

You should see something like:

```
mysql Ver 8.0.26-0ubuntu0.20.04.2 for Linux on x86_64 ((Ubuntu))
```

---

### Secure the MySQL Server

You may want to run the included security script to improve MySQL’s security. This will change default options for remote root logins and sample users.

Start the MySQL server:

```bash
sudo /etc/init.d/mysql start
```

Then run the security script:

```bash
sudo mysql_secure_installation
```

This script will ask you to set a root password, remove anonymous users, and more.

**Note:** The above settings are okay for local development. For production, secure the root user with a strong password and prevent external access.

---

### Verify MySQL is Running

To open the MySQL prompt, run:

```bash
sudo mysql
```

To view the available databases, use the command:

```sql
SHOW DATABASES;
```

You should see output like:

| Database          |
|-------------------|
| information_schema|
| mysql             |
| performance_schema|
| sys               |

---

### Allow Remote Access

To allow remote access to the MySQL server, run the following commands inside the MySQL prompt:

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
```

Exit MySQL:

```sql
exit
```

---

### Configure HeidiSQL

You can configure HeidiSQL to connect to MySQL 8.0 on WSL 2 by following these steps:

1. Click **New** and name the connection (e.g., `MySQL-WSL2`).
2. Set **Library** to `libmysql.dll`.
3. Hostname/IP: `localhost`.
4. User: `root`.
5. Password: `root`.

---

### Change the Default Port

If MySQL running in Windows and MySQL on Ubuntu are using the same port (default 3306), you’ll need to change one of the ports to avoid conflicts.

To change MySQL’s port, edit the configuration file:

```bash
sudo nano /etc/mysql/my.cnf
```

Add the following lines to change the port:

```ini
[mysqld]
port = 33061
```

Save and exit the file (CTRL+W, Enter, CTRL+X).

Restart MySQL:

```bash
sudo service mysql restart
```

Make sure to update HeidiSQL or any `.env` files to reflect the new port.

---

### Start MySQL Automatically When WSL Starts

To make MySQL start automatically when WSL starts, run:

```bash
sudo update-rc.d mysql defaults
```

---

### Manually Start MySQL

If you don’t need MySQL to start every time WSL starts, you can manually start it with:

```bash
sudo service mysql start
```

---

### Troubleshooting

#### Typical Errors and Possible Reasons

1. **Error when MySQL is not running**: Make sure the service is running.
2. **Access Denied Errors**: Ensure you're using the correct password and user privileges.

#### Basic Troubleshooting

1. Make sure WSL is started.
2. Check that MySQL server is running.
3. Refer to [How to fix common problems with MySQL databases](https://upcloud.com/community/tutorials/fix-common-problems-mysql-databases/).

#### Check the Server is Running

To check if MySQL is running, use:

```bash
sudo service mysql status
```

If it’s not running, restart the service:

```bash
sudo service mysql restart
```

#### Check the MySQL Server for Errors

To check MySQL’s error logs:

```bash
sudo grep -i error /var/log/mysql/error.log
```

#### Check the Port

To verify the port MySQL is running on:

```bash
mysql -u root -p
```

Then, inside the MySQL prompt:

```sql
SHOW GLOBAL VARIABLES LIKE 'PORT';
```

---

### Stop MySQL and WSL

To exit the WSL terminal:

```bash
exit
```

To shut down WSL from PowerShell:

```bash
wsl --shutdown
```

---

