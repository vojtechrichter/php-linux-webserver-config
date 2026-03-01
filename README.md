Ansible playbook for setting up a linux server with PHP-FPM (8.4), Nginx, and PostgreSQL

## Usage
## Configure the target server
Edit `inventory/hosts.ini`:
```ini
[vps]
YOUR_SERVER_IP ansible_user=root ansible_ssh_private_key_file=~/.ssh/id_rsa
```

## Set variables
Edit `vars.yml` - at minimum change:
- `deploy_user_password`
- `postgres_users[*].password`
- Any users/databases that should be created

### Run the playbook
`ansible-playbook site.yml`

Or dry run:
`ansible-playbook site.yml --check`


## Adding a new website
### Create the site config:
`cp /etc/nginx/sites-available/example.com /etc/nginx/sites-available/yourdomain.com`
`nano /etc/nginx/sites-available/example.com`

Copy the config from `roles/nginx/files/example-site.conf`

### Enable the site
`ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/`
`nginx -t && systemctl reload nginx`

### Obtain a SSL certificate
`apt install certbot python3-certbot-nginx`
`certbot --nginx -d example.com -d www.example.com`
