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
 
 <br />

 
 ## Access REST API
 
<strong>Get service root</strong>
The service root is your starting point to discover the REST API. It contains the basic starting points for the database, and some version and extension information.

Example request:

```
GET http://localhost:7474/db/data/
Accept: application/json; charset=UTF-8
Example response

200: OK
Content-Type: application/json;charset=utf-8
```

<strong>RESTful Python example to CREATE a store and a customer with Cypher (CQL)</strong>

```
create_my_store_with_customer = {
    "statements": [
        {
            "statement": 'MERGE (customer:Customer { customer_id:"1", customer_name:"mary" })' +
            'MERGE (store:MyStore { my_store: "El Bodega" })' +
            'MERGE (store)-[:store_relationship_to]->(customer)' +
            'RETURN customer, store'
        }
    ]
}

url = "http://localhost:7474/db/neo4j/tx/commit"
headers = {
    'Content-Type': 'application/json',
    'Accept': 'application/json;charset=UTF-8',
}

r = requests.request("POST", url, data=json.dumps(create_my_store_with_customer), headers=headers)
print(r.text)

```
 
Addt'l documentation on the REST API can be found here: ```https://neo4j.com/docs/rest-docs/3.5/```

 
--------------------------------------------------------------------

 
 ## Add Addt'l users

<strong>1. Add a user</strong>

The following commands can be entered inside the NEO4J UI where you eneter your Cypher queries.

1. The following example creates a user with the username 'johnsmith' and password 'h6u4%kr'. When the user 'johnsmith' logs in for the first time, he will be required to change his password. 

```CALL dbms.security.createUser('johnsmith', 'h6u4%kr', true)```

<strong>2. Delete a user</strong>

The following example deletes a user with the username 'janebrown'.

```CALL dbms.security.deleteUser('janebrown')```
