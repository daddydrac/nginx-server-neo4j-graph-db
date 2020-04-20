## NGINX Server for NEO4J Graph DB
Nginx server for NEO4J Graph Database and NEO4J REST API

#### Configuration directions

<em>Do this prior to running the container command:</em>

 - Open up ports on your servers firewall that match nginx.conf and docker-compose file(s)
 - Change ```<hostname>``` in nginx.conf to your hostname. 
 - Create a folder named ```ssl``` inside ./nginx folder, & generate SSL certs:
    - Open the command line and run these commands inside the ssl folder to generate a self signed certificate:
    
      ```openssl req -new -newkey rsa:4096 -x509 -sha512 -days 365 -nodes -out nginx.crt -keyout nginx.key```
      
    - Generate the DH Params: With Forward Secrecy, if an attacker gets a hold of the server's private key, it will not be able to decrypt past communications. The private key is only used to sign the DH handshake, which does not reveal the pre- master key. Diffie-Hellman ensures that the pre-master keys never leave the client and the server, and cannot be intercepted by a MITM. This takes bout 3-5 min to generate:
    
      ```openssl dhparam -out dhparam.pem 4096```
 
 
 1. Go into the root folder and run:
 
 ``` docker-compose up -d --build ```
 
 2. https is avail on: 
 
 ```https://<hostname>/browser/```
 
 --------------------------------------------------------------------
 
 #### Add Addt'l users

<strong>1. Add a user</strong>

The following commands can be entered inside the NEO4J UI where you eneter your Cypher queries.

1. The following example creates a user with the username 'johnsmith' and password 'h6u4%kr'. When the user 'johnsmith' logs in for the first time, he will be required to change his password. 

```CALL dbms.security.createUser('johnsmith', 'h6u4%kr', true)```

<strong>2. Delete a user</strong>

The following example deletes a user with the username 'janebrown'.

```CALL dbms.security.deleteUser('janebrown')```
