# Port Management with Apache on Ubuntu

Hello community, here is how to manage ports when you have multiple applications deployed using Apache server on Ubuntu.

## Step 1: Configure Apache Virtual Host for Django, laravel, flask or any

First, edit the Apache configuration file for your Django project:

```bash
sudo nano /etc/apache2/sites-available/django_project.conf
```

-- add the following code

```python
<VirtualHost *:8000>
    ServerAdmin admin@management
    ServerName 10.0.2.15

    DocumentRoot /home/management/SUPERMARKET/project

    Alias /static /home/management/SUPERMARKET/project/static
    <Directory /home/management/SUPERMARKET/project/static>
        Require all granted
    </Directory>

    <Directory /home/management/SUPERMARKET/project>
        <Files wsgi.py>
            Require all granted
        </Files>
    </Directory>

    WSGIDaemonProcess supermarket python-path=/home/management/SUPERMARKET/project python-home=/home/management/SUPERMARKET/project/venv
    WSGIProcessGroup supermarket
    WSGIScriptAlias / /home/management/SUPERMARKET/project/project/wsgi.py

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
## Step 2 :Next, edit the ports configuration file to add all the ports you will use for different applications
 
```bash
sudo gedit /etc/apache2/ports.conf
```
- Add the following lines to specify the ports:
```python
# Email server, pgadmin or any
Listen 80

# Django app
Listen 8000

# Laravel
Listen 8001

# Flask
Listen 5000

# Streamlit
Listen 8501

# pgAdmin
Listen 5050

<IfModule ssl_module>
    Listen 443
</IfModule>

<IfModule mod_gnutls.c>
    Listen 443
</IfModule>
```

## step 3:  restart server
```sh
sudo systemctl restart apache2
```
