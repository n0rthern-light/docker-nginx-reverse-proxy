# docker-nginx-reverse-proxy
A reverse proxy implementation using Nginx to handle multiple docker containers running on the same 443 port.

### Make sure to setup your local environemnt domains correclty in /etc/hosts on your machine.

The SSL certs are broken and are just used to force HTTPS in the proxy, if you need valid ones you need to generate them yourself and setup them in the proxy default.conf - separately for each domain. In the current setup all domains are sharing 1 broken SSL certificate - hacky way.

## How to create authorized self-signed certificate:
### Credits: JellicleCat

```
######################
# Become a Certificate Authority
######################

# Generate private key
openssl genrsa -des3 -out myCA.key 2048
# Generate root certificate
openssl req -x509 -new -nodes -key myCA.key -sha256 -days 825 -out myCA.pem

######################
# Create CA-signed certs
######################

NAME=mydomain.com # Use your own domain name
# Generate a private key
openssl genrsa -out $NAME.key 2048
# Create a certificate-signing request
openssl req -new -key $NAME.key -out $NAME.csr
# Create a config file for the extensions
>$NAME.ext cat <<-EOF
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names
[alt_names]
DNS.1 = $NAME # Be sure to include the domain name here because Common Name is not so commonly honoured by itself
DNS.2 = bar.$NAME # Optionally, add additional domains (I've added a subdomain here)
IP.1 = 192.168.0.13 # Optionally, add an IP address (if the connection which you have planned requires it)
EOF
# Create the signed certificate
openssl x509 -req -in $NAME.csr -CA myCA.pem -CAkey myCA.key -CAcreateserial \
-out $NAME.crt -days 825 -sha256 -extfile $NAME.ext

```
- Become a CA
- Sign your certificate using your CA cert+key
- Import myCA.pem as an "Authority" (not into "Your Certificates") in your Chrome settings (Settings > Manage certificates > Authorities > Import)
- Use the $NAME.crt and $NAME.key files in your server
