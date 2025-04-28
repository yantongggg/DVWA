
# DVWA Installation Guide

This guide provides the full setup steps to install and configure the Damn Vulnerable Web Application (DVWA) on Kali Linux.

---

## 1. Cloning DVWA Repository

Change directory to the web root:
```bash
cd /var/www/html
```

Clone the DVWA repository (requires root permissions):
```bash
sudo git clone https://github.com/digininja/DVWA.git
```

---

## 2. Setting Proper Permissions

Set full permissions for the DVWA folder:
```bash
sudo chmod -R 777 DVWA
```

---

## 3. Configuring DVWA

Navigate into the DVWA folder:
```bash
cd DVWA
```

Go to the `config` directory:
```bash
cd config
```

Copy the sample config file:
```bash
cp config.inc.php.dist config.inc.php
```

Edit the configuration file:
```bash
sudo nano config.inc.php
```

Update the database configuration fields:
```php
$_DVWA['db_server'] = '127.0.0.1';
$_DVWA['db_database'] = 'dvwa';
$_DVWA['db_user'] = 'dvwa';
$_DVWA['db_password'] = 'p@ssw0rd';
$_DVWA['db_port'] = '3306';
```

---

## 4. Setting up the Database

Start the MySQL service:
```bash
sudo systemctl start mysql
```

Check MySQL status:
```bash
sudo systemctl status mysql
```

Login to MySQL:
```bash
mysql -u root -p
```
(Press Enter if no password is set.)

Inside MySQL:
```sql
CREATE DATABASE dvwa;
CREATE USER 'dvwa'@'127.0.0.1' IDENTIFIED BY 'p@ssw0rd';
GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwa'@'127.0.0.1';
FLUSH PRIVILEGES;
EXIT;
```

---

## 5. Configuring PHP Settings

Navigate to your PHP configuration for Apache:
```bash
cd /etc/php/8.2/apache2
```

Edit the `php.ini` file:
```bash
sudo nano php.ini
```

Make sure these two settings are enabled:
```ini
allow_url_fopen = On
allow_url_include = On
```

Save and exit.

---

## 6. Restart Services

Restart Apache2 to apply changes:
```bash
sudo systemctl restart apache2
```

---

## 7. Accessing DVWA

Open your browser and visit:
```
http://127.0.0.1/DVWA/login.php
```

Login credentials:
- **Username:** `admin`
- **Password:** `password`

If you need to reset the database:
```
http://127.0.0.1/DVWA/setup.php
```
Follow the setup instructions.

---

# ✅ Done!

DVWA is now installed and ready for safe, local vulnerability testing.  
> **Note:** Only use DVWA in a controlled lab environment!
