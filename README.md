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
