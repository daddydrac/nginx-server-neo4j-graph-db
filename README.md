## NGINX Server for NEO4J Graph DB
Nginx server for NEO4J Graph Database and NEO4J REST API

NOTE - This is a WIP and only works on http as of 4/19/20

Steps to reproduce:

 - Open up ports on your servers firewall that match nginx.conf and docker-compose file(s)
 - Change ```<hostname>``` in nginx.conf to your hostname. 
 - Generate SSL certs:
    - Open the command line and run these commands inside the ssl folder to generate a self signed certificate:
    
      ```openssl req -new -newkey rsa:4096 -x509 -sha512 -days 365 -nodes -out nginx.crt -keyout nginx.key```
      
    - Generate the DH Params: With Forward Secrecy, if an attacker gets a hold of the server's private key, it will not be able to decrypt past communications. The private key is only used to sign the DH handshake, which does not reveal the pre- master key. Diffie-Hellman ensures that the pre-master keys never leave the client and the server, and cannot be intercepted by a MITM. This takes bout 3-5 min to generate:
    
      ```openssl dhparam -out dhparam.pem 4096```
 
 
 Run in root:
 
 ``` docker-compose up -d --build ```
 
 http is avail on:
 
 ```http://<DNS_SERVER_NAME>:7474/browser/```
 
 https is avail on: 
 
 ```https://<DNS_SERVER_NAME>/browser/```
