# configuration
<VirtualHost *:443>
    ServerName ai-being.com
    ServerAdmin support@aibeing.com

    WSGIDaemonProcess AIBEING python-path=/lib/python3.8/ threads=50 inactivity-timeout=60
    WSGIProcessGroup AIBEING
    WSGIScriptAlias / /var/www/AIBEING/aibeing.wsgi
    Alias /static /var/www/AIBEING/static
    <Directory /var/www/AIBEING/static/>
        Require all granted
    </Directory>

    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/ai-being.com/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/ai-being.com/privkey.pem
</VirtualHost>

# WSGI
sudo apt install python3-dev
pip install python-ldap
sudo apt install libapache2-mod-wsgi-py3


# CERT BOT
sudo apt update
sudo apt install certbot python3-certbot-apache
sudo certbot --apache -d yourdomain.com -d www.yourdomain.com
sudo certbot renew --dry-run
sudo systemctl restart apache2


sudo a2enmod ssl
sudo a2ensite 000-default-le-ssl.conf
sudo apache2ctl configtest
sudo systemctl restart apache2
<VirtualHost *:80>
    ServerName ai-being.com
    ServerAdmin support@aibeing.com
    Redirect permanent / https://ai-being.com/
</VirtualHost>


# Server Configuration
Step 1: Backup Databases on the Source Server
SSH into the source server:

ssh user@source-server-ip
Dump all databases:

mysqldump -u root -p --all-databases > all_databases.sql
Transfer the dump file to the destination server:

scp all_databases.sql user@destination-server-ip:/path/to/directory
Step 2: Restore Databases on the Destination Server
SSH into the destination server:

ssh user@destination-server-ip
Restore the dump:

mysql -u root -p < /path/to/directory/all_databases.sql
Step 3: Verify the Databases
Check if databases are correctly transferred:

mysql -u root -p -e "SHOW DATABASES;"
