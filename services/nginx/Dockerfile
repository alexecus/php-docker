FROM nginx:1.15

# mark volume/s as externally mounted
VOLUME ["/var/www", "/etc/nginx/conf.d", "/var/log/nginx"]

# starting point
WORKDIR /var/www

CMD ["/usr/sbin/nginx"]
