# Deployment Lab1

Django website deployment with apache2 at AWS EC2 instance

## Commands at instance
    
```bash
sudo apt-get update
sudo apt-get install python3-pip apache2 libapache2-mod-wsgi-py3
sudo pip install virtualenv

mkdir ~/apps
cd ~/apps

git clone https://github.com/hariomdev/deployment_lab1.git

cd deployment_lab1

virtualenv .venv
source myprojectenv/bin/activate

pip install -r reuirements.txt
```

## Apache site configuration

```bash
cd /etc/apache2/sites-available

nano you_site_name.conf

and copy and past below configuration script

sudo a2dissite 000-default.conf

sudo a2ensite you_site_name.conf
```
    
## Configuration Script

```bash
<VirtualHost *:80>
    ServerAdmin EMAIL_ADDRESS_OF_ADMIDISTRATOR
    ServerName ADD_YOUR_DOAMIN_ADDRESS
    ServerAlias ADD_YOUR_DOAMIN_ADDRESS

    DocumentRoot /home/ubuntu/apps/deployment_lab1

    Alias /static /home/ubuntu/apps/deployment_lab1/static
    <Directory /home/ubuntu/apps/deployment_lab1/static>
        Require all granted
    </Directory>

    <Directory /home/ubuntu/apps/deployment_lab1>
        Options Indexes FollowSymLinks Includes ExecCGI
        AllowOverride All
        Order allow,deny
        Allow from all
        Require all granted
    </Directory>

    <Directory /home/ubuntu/apps/deployment_lab1>
        <Files wsgi.py>
            AllowOverride All
            Order allow,deny
            Allow from all
            Require all granted
        </Files>
    </Directory>

    WSGIDaemonProcess deployment_lab1 python-path=/home/ubuntu/apps/deployment_lab1 python-home=/home/ubuntu/apps/deployment_lab1/.venv
    WSGIProcessGroup deployment_lab1
    WSGIScriptAlias / /home/ubuntu/apps/deployment_lab1/deployment_lab1/wsgi.py

</VirtualHost>
```
## Thanks