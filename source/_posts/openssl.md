# openssl




Create 

```
openssl genrsa -out server.key 2048
openssl req -new -key server.key -out server.csr
openssl x509 -req -in server.csr -out server.crt -signkey server.key -days 3650
python3 manage.py runserver_plus --cert-file ./server.crt --key-file ./server.key 0.0.0.0:8000
```


1. Create a Certificate Authority private key (this is your most important key):

`openssl req -new -newkey rsa:1024 -nodes -out ca.csr -keyout ca.key`

2. Create your CA self-signed certificate:

`openssl x509 -trustout -signkey ca.key -days 365 -req -in ca.csr -out ca.pem`

3. Issue a client certificate by first generating the key, then request (or use one provided by external system) then sign the certificate using private key of your CA:
```
openssl genrsa -out client.key 1024
openssl req -new -key client.key -out client.csr
openssl ca -in client.csr -out client.cer
```




#### Manually stopping Pacific keycloak
`ps -ef | grep "/opt/keycloak" | awk '{print $2}' | xargs kill`