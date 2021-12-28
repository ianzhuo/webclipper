# A simple setup for self-hosted WordPress on Docker with SSL - HackMD
###### [](#tags-c4lab "tags-c4lab")tags: `c4lab`

## [](#First-step-install-it "First-step-install-it")First step, install it

Follow [https://hub.docker.com/\_/wordpress/](https://hub.docker.com/_/wordpress/) to make sure WordPress can work on your machine.

In this tutorial, `wordpress:5.4.0-php7.2-apache` are used.

## [](#Second-get-certification "Second-get-certification")Second, get certification

You can use certbot to get certification files

    docker run --rm -it -p 80:80 -p 443:443 -v $PWD/letsencrypt:/etc/letsencrypt certbot/certbot certonly --standalone 

Then copy the certification files out.

    cp letsencrypt/live/my.domain.ntu.edu.tw/* certs/ 

or follow others tutorial if it doesn’t work.

## [](#First-set-up-HTTPS "First-set-up-HTTPS")First, set up HTTPS

Mount some modified files into container.

```null
  wordpress:
    image: wordpress:5.4.0-php7.2-apache
    restart: always
    environment:
    WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: exampleuser
      WORDPRESS_DB_PASSWORD: examplepass
      WORDPRESS_DB_NAME: exampledb
    volumes:
      - ./wordpress:/var/www/html
      - ./certs:/etc/ssl/certs:ro
      - ./default-ssl.conf:/etc/apache2/sites-available/default-ssl.conf:ro
      - ./docker-entrypoint.sh:/usr/local/bin/docker-entrypoint.sh:ro
    ports:
      - 443:443
...
(Did not change other things)
```

-   `./certs` is the folder your certs are.
-   `./default-ssl.conf` is the configuration of port 443 on Apache, recommended to set as follow.

    ```null
    <VirtualHost *:443>
      DocumentRoot /var/www/html
      ErrorLog ${APACHE_LOG_DIR}/error.log
      CustomLog ${APACHE_LOG_DIR}/access.log combined
      SSLEngine on
      ServerName my.domain.ntu.edu.tw  
      SSLCertificateFile  /etc/ssl/certs/fullchain.pem  
      SSLCertificateKeyFile /etc/ssl/certs/privkey.pem  
      <FilesMatch "\.(cgi|shtml|phtml|php)$">
              SSLOptions +StdEnvVars
      </FilesMatch>
      <Directory /usr/lib/cgi-bin>
              SSLOptions +StdEnvVars
      </Directory>
    </VirtualHost>
    ```
-   `docker-entrypoint.sh` will run before the web server started.

    1.  Copy this file from Image. (`wordpress_wordpress_1` is the container name which wordpress is on, feel free to change it)

        ```
        docker cp wordpress_wordpress_1:/usr/local/bin/docker-entrypoint.sh .

        ```
    2.  Then, change last line of `docker-entrypoint.sh`.\`\`\`null
        a2enmod ssl
        a2ensite default-ssl
        service apache2 restart
        service apache2 stop
        exec "$@"
        ```

        ```

## [](#Finally-Try-it "Finally-Try-it")Finally, Try it

## [](#Advance "Advance")Advance

If you want to split Wordpress main program into FPM and NGINX.

See this repo [https://github.com/dbtek/docker-compose-wordpress-fpm-nginx](https://github.com/dbtek/docker-compose-wordpress-fpm-nginx) 
 [https://hackmd.io/@linnil1/H1p25uxFU](https://hackmd.io/@linnil1/H1p25uxFU)
