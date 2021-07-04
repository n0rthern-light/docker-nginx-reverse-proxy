# docker-nginx-reverse-proxy
A reverse proxy implementation using Nginx to handle multiple docker containers running on the same 443 port.

### Make sure to setup your local environemnt domains correclty in /etc/hosts on your machine.

The SSL certs are broken and are just used to force HTTPS in the proxy, if you need valid ones you need to generate them yourself and setup them in the proxy default.conf - separately for each domain. In the current setup all domains are sharing 1 broken SSL certificate - hacky way.
