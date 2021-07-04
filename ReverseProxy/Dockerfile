FROM nginx:alpine

COPY ./certs/key.key /etc/nginx/ssl/key.key
COPY ./certs/cert.cert /etc/nginx/ssl/cert.cert

#  default conf for proxy service
COPY ./default.conf /etc/nginx/conf.d/default.conf

# NOT FOUND response
COPY ./404.html /var/www/html/404.html