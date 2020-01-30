# How to install php7.1-fpm with EasyEngine
--------
### Install php7.1-fpm
```
apt install php7.1-common php7.1-cli php7.1-zip php7.1-opcache php7.1-mysql php7.1-mcrypt php7.1-mbstring php7.1-json php7.1-intl php7.1-gd php7.1-fpm php7.1-curl php7.1-bz2
```

**Copy the php7.1-fpm pool configuration from php7.0-fpm**
```
cp -f /etc/php/7.0/fpm/pool.d/www.conf /etc/php/7.1/fpm/pool.d/www.conf
```

**Edit the listening port of php7.1-fpm (for example 7080 instead of 7070)**
```
nano /etc/php/7.1/fpm/pool.d/www.conf
```
Replace the line `listen = 127.0.0.1:9070` by `listen = 127.0.0.1:9080`<br>
Restart the service
```
service php7.1-fpm restart
```
Then to use php7.1-fpm, you have the choice between 

--------

### 1) Add php7.1-fpm as an additional php version

Add the following lines in /etc/nginx/conf.d/upstream.conf
```
upstream php71 {
server 127.0.0.1:9080;
}
```

then copy the files /etc/nginx/common/php7.conf into  /etc/nginx/common/php71.conf<br>
```
cp -f /etc/nginx/common/php7.conf /etc/nginx/common/php71.conf
```

And into this copy replace the line `fastcgi_pass php7;` by `fastcgi_pass php71;`<br>
```
vim /etc/nginx/common/php71.conf
```

Reload nginx and you can replace the line `include common/php7.conf;` by `include common/php71.conf;` in the vhosts of your choice<br>

```
vim /etc/nginx/sites-available/$domain
```

Nice now Restart PHP7.1
```
service php7.1-fpm restart 
```
