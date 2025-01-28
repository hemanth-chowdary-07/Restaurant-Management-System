# Food Ordering System - Installation Guide

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [System Requirements](#system-requirements)
3. [Installation Steps](#installation-steps)
4. [Database Setup](#database-setup)
5. [Web Server Configuration](#web-server-configuration)
6. [Application Setup](#application-setup)
7. [Testing](#testing)
8. [Troubleshooting](#troubleshooting)
9. [Default Credentials](#default-credentials)
10. [Security Considerations](#security-considerations)

## Prerequisites

Before installation, ensure you have:
- Linux server/WSL with Ubuntu
- Root or sudo access
- Terminal access
- Internet connection

## System Requirements

The system requires:
- Apache2 (2.4+)
- PHP 8.1 or higher
- MySQL 8.0 or higher
- Required PHP extensions:
  - mysqli
  - mysql
  - session
  - gd2
  - mbstring

## Installation Steps

### 1. Update System Packages
```bash
sudo apt update
sudo apt upgrade
```

### 2. Install Required Packages
```bash
sudo apt install apache2 php mysql-server php-mysql libapache2-mod-php php-gd php-cli php-common
```

### 3. Install PHP Extensions
```bash
sudo apt install php8.1-mysqli php8.1-mysql php8.1-gd php8.1-mbstring
```

## Database Setup

### 1. Secure MySQL Installation
```bash
sudo mysql_secure_installation
```

### 2. Create Database and User
```bash
sudo mysql -u root -p
```

Run these MySQL commands:
```sql
CREATE DATABASE food;
CREATE USER 'fooduser'@'localhost' IDENTIFIED BY 'your_secure_password';
GRANT ALL PRIVILEGES ON food.* TO 'fooduser'@'localhost';
FLUSH PRIVILEGES;
exit;
```

### 3. Import Database Schema
```bash
sudo mysql -u root -p food < food.sql
```

## Web Server Configuration

### 1. Configure Apache
```bash
sudo mkdir -p /var/www/html/food
sudo chown -R $USER:$USER /var/www/html/food
```

### 2. Enable Required Apache Modules
```bash
sudo a2enmod php8.1
sudo a2enmod rewrite
sudo service apache2 restart
```

## Application Setup

### 1. Copy Application Files
```bash
# Navigate to the downloaded application folder
cd food-ordering-system
sudo cp -r * /var/www/html/food/
```

### 2. Set File Permissions
```bash
sudo chown -R www-data:www-data /var/www/html/food
sudo chmod -R 755 /var/www/html/food
sudo chmod -R 777 /var/www/html/food/images
```

### 3. Configure Database Connection
Edit the database connection file:
```bash
sudo nano /var/www/html/food/includes/connect.php
```

Update with your database credentials:
```php
<?php
session_start();
$servername = "localhost";
$server_user = "fooduser";
$server_pass = "your_secure_password";
$dbname = "food";
$con = new mysqli($servername, $server_user, $server_pass, $dbname);
?>
```

## Testing

1. Access the application:
```
http://localhost/food
```

2. Test both customer and admin interfaces using default credentials.

## Default Credentials

### Administrator Account:
- Username: root
- Password: toor

### Customer Account:
- Username: user1
- Password: pass1

## Security Considerations

1. Change default passwords immediately after installation
2. Secure the includes directory
3. Regularly update system packages
4. Configure SSL/TLS for production use
5. Set proper file permissions
6. Use strong passwords for MySQL

## Troubleshooting

### Common Issues and Solutions

1. **500 Internal Server Error**
   - Check Apache error logs:
   ```bash
   sudo tail -f /var/log/apache2/error.log
   ```
   - Verify PHP version compatibility
   - Check file permissions

2. **Database Connection Issues**
   - Verify MySQL service is running:
   ```bash
   sudo service mysql status
   ```
   - Check database credentials
   - Confirm MySQL user permissions

3. **White Screen / Blank Page**
   - Enable PHP error reporting in php.ini
   - Check PHP log files
   - Verify PHP extensions are installed

4. **File Permission Issues**
   ```bash
   sudo chown -R www-data:www-data /var/www/html/food
   sudo chmod -R 755 /var/www/html/food
   ```

## Additional Notes

- The system includes a virtual wallet system
- New customers get 2000 coins in their wallet by default
- Only verified customers can use "Cash on Delivery"
- Admin can manage users, orders, and menu items
- The system includes a ticket system for customer support

## Wallet System

- Each new user gets a default balance of 2000 coins
- Virtual card numbers are generated automatically
- Wallet can be recharged by admin
- Transaction history is maintained