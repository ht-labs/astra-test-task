## Table of contents
* [General info](#general-info)
* [Technologies](#technologies)
* [Setup](#setup)
* [Change log](#change-log)

## General info
This project is an entry test task for Astra.
	
## Technologies
Project is created with:
* Terraform v1.3.7
* Ansible v2.13.7
* Nginx v1.23.3

## Setup

### Requirements for host machine
1. Terraform v1.3.7+
2. Ansible v2.13.7+

### Requirements for Yandex Cloud
1. Valid subscription 
2. Valid Service Account
3. Static IP address

### Setup steps

1. Create ip.txt file in the following format, by specifying your static YC IP address
```txt
<xx.xx.xx.xx>
```

1.1. Add the same IP into your Ansible inventory

2. Create user.txt file using the following template
```txt
#cloud-config
users:
  - name: <service account name>
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh_authorized_keys:
    - <key name> <pub key> <local-user@host-machine>
```

3. Generate (or obtain otherwise) SSL certificate
3.1. Place the .crt file into <project_dir>/ssl/certs/ directory
3.2. Place the .key file into <project_dir>/ssl/private/ directory
3.3. Add your certificate information into /nginx/conf.d/astra.conf file by updating placeholders

WARNING! 
DO NOT EXPOSE SSL CERTIFICATE FILES

4. Run main.tf Terraform project file from the project directory
```bash
terraform apply
```

5. After completion, run Ansible playbook, playbook-docker.yml under your YC service account name
```bash
ansible-playbook -i inventory playbook-docker.yml -u <Service Account Name>
```

6. Go to your hostname and IP address in the web-browser to verify that nginx starting page is accessible via https

## Change log

v0.3.1 - Readme header formatting fix

v0.3.0 - added nginx Docker container into VM, configured to work with SSL certificates via HTTPS 

v0.2.1 - added Setup section into README.md, explaining the project usage steps

v0.2.0 - created Ansible playbook with basic initialization and UFW configuration tasks

v0.1.0 - created Terraform project file
