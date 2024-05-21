# Ansible Role: Passbolt

An Ansible role for automating the installation and configuration of Passbolt, an open-source password manager, on Ubuntu 20.04.

## Table of Contents

- [Features](#features)
- [Requirements](#requirements)
- [Role Structure](#role-structure)
- [Dependencies](#dependencies)
- [Installation](#installation)
- [Configuration](#configuration)
- [SSL/TLS Support](#ssltls-support)
- [Vagrant Integration](#vagrant-integration)
- [Usage](#usage)
  - [Example Playbook](#example-playbook)
  - [Custom Variables](#custom-variables)
- [Testing](#testing)
- [Troubleshooting](#troubleshooting)
- [Resources](#resources)
- [License](#license)

## Features

- **Automated Provisioning**: Seamlessly provision Passbolt on Ubuntu 20.04 VMs.
- **Dependency Management**: Automatically handle installation of necessary dependencies such as Nginx, PHP, and MySQL/PostgreSQL.
- **Flexible Configuration**: Easily customize Passbolt settings, including database connection details and security configurations.
- **SSL/TLS Support**: Optionally enable SSL/TLS for secure communication with the Passbolt instance.
- **Automated Backups**: Set up automated backups for Passbolt data.
- **Vagrant Integration**: Integrate with Vagrant for local testing and development.

## Requirements

- Ansible 2.9+
- Ubuntu 20.04

## Role Structure

passbolt_role/
├── defaults/
│ └── main.yml
├── handlers/
│ └── main.yml
├── meta/
│ └── main.yml
├── tasks/
│ ├── dependencies.yml
│ ├── main.yml
│ ├── passbolt_config.yml
│ └── passbolt_install.yml
├── templates/
│ ├── passbolt.conf.j2
│ ├── passbolt.php.j2
│ └── php.ini.j2
├── vars/
│ └── main.yml
├── tests/
│ └── test.yml
└── README.md


## Dependencies

This role relies on the following dependencies:
- Nginx
- PHP-FPM
- PHP-MySQL
- PHP-Gnupg
- MySQL Server

These dependencies are automatically installed by the role.

## Installation

To install this role, clone the repository into your Ansible roles directory:

git clone https://github.com/your-username/passbolt_role.git /etc/ansible/roles/passbolt_role


## Configuration

The role includes default variables that can be customized as needed. These variables are located in defaults/main.yml and can be overridden in your playbook.

Default Variables

# Database settings
passbolt_db_name: passbolt
passbolt_db_user: passbolt_user
passbolt_db_password: example_password

# Passbolt settings
passbolt_base_url: https://passbolt.example.com
passbolt_gpg_private_key_path: /path/to/private.key

# Web server settings
nginx_config_file: /etc/nginx/sites-available/passbolt

# PHP settings
php_config_file: /etc/php/7.4/fpm/php.ini

# Backup settings
backup_directory: /var/backups
backup_cron_schedule: "0 3 * * *"

# SSL/TLS Support
To enable SSL/TLS, customize the Nginx configuration template (templates/passbolt.conf.j2) and add your SSL certificate and key paths:

server {
    listen 443 ssl;
    server_name passbolt.example.com;

    ssl_certificate /etc/nginx/ssl/passbolt.crt;
    ssl_certificate_key /etc/nginx/ssl/passbolt.key;

    root /var/www/passbolt/webroot;
    index index.php;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.4-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}

# Vagrant Integration
To integrate with Vagrant, create a Vagrantfile and reference the Ansible role in your Vagrant provisioner configuration:

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.roles_path = "roles"
  end
end

# Usage
## Example Playbook
Create a playbook (playbook.yml) to use the role:

- hosts: servers
  roles:
    - passbolt_role

## Custom Variables
Override default variables in your playbook or inventory:

- hosts: servers
  vars:
    passbolt_db_name: my_passbolt_db
    passbolt_db_user: my_passbolt_user
    passbolt_db_password: my_secure_password
    passbolt_base_url: https://my-passbolt.example.com
  roles:
    - passbolt_role

# Testing
Automated tests are included in the tests/ directory. To run the tests:

Install dependencies: ansible-galaxy install -r requirements.yml
Execute the test playbook: ansible-playbook tests/test.yml

# Troubleshooting

Issue: Passbolt installation fails.
Solution: Ensure all dependencies are installed and the server has internet access to download Passbolt.

Issue: Nginx configuration errors.
Solution: Verify the Nginx configuration template for syntax errors and correct paths.

Issue: Database connection errors.
Solution: Check database credentials and ensure the database server is running.


# Resources
Passbolt Documentation ()
Ansible Documentation ()
Nginx Documentation ()
PHP Documentation ()
MySQL Documentation ()
