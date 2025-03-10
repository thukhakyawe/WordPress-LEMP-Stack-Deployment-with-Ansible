# WordPress LEMP Stack Deployment with Ansible

## Overview
This repository contains an Ansible playbook to deploy WordPress on a LEMP (Linux, Nginx, MySQL, PHP) stack. The playbook automates the setup of the required packages, MySQL database configuration, and Nginx web server configuration.

## Prerequisites
Before running the playbook, ensure you have the following:

- Ansible installed on your local machine.
- A remote server with Ubuntu.
- Your SSH public key (`id_rsa.pub`) added to `/home/ubuntu/.ssh/authorized_keys` on the remote server.

## Files in This Repository
- `inventory.ini` - Defines the remote hosts for Ansible.
- `wordpress-lemp.yml` - The main Ansible playbook for deploying WordPress.
- `templates/wordpress_nginx.conf.j2` - Jinja2 template for configuring Nginx.
- `readme.txt` - Instructions on running the playbook.

## Inventory Configuration
Modify `inventory.ini` with your server's IP address:

```
[webservers]
your_server_ip ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa
```

## Running the Playbook
1. Clone this repository:
   ```sh
   git clone https://github.com/thukhakyawe/WordPress-LEMP-Stack-Deployment-with-Ansible.git
   cd 
   ```

2. Update the `inventory.ini` file with your server details.

3. Run the playbook:
   ```sh
   ansible-playbook -i inventory.ini wordpress-lemp.yml
   ```

## Nginx Configuration
The Nginx configuration template `templates/wordpress_nginx.conf.j2` is used to configure the web server:

```nginx
server {
    listen 80;
    server_name your_server_ip;
    
    root /var/www/wordpress;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.3-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|eot|ttf|otf|ttc|mp4|webm|ogv|ogg|mp3|wav|flac|aac|m4a)$ {
        expires max;
        log_not_found off;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

## MySQL Configuration
The playbook sets up MySQL with the following variables:

```yaml
vars:
  mysql_root_password: "your_mysql_root_password"
  mysql_db_name: "wordpress"
  mysql_user: "wp_user"
  mysql_password: "your_wp_password"
```

Ensure you replace these with secure values before running the playbook.

## Troubleshooting
- Ensure your SSH private key (`~/.ssh/id_rsa`) has the correct permissions (`chmod 600 ~/.ssh/id_rsa`).
- Verify Ansible is installed with `ansible --version`.
- If there are MySQL connection issues, ensure MySQL is running: `sudo systemctl status mysql`.
- Check Nginx configuration: `sudo nginx -t` and restart if needed: `sudo systemctl restart nginx`.

---

Enjoy your automated WordPress deployment with Ansible! 🚀

