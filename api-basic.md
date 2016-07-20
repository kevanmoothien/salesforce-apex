Salesforce Api basics!
===================

## Common types of Salesforce API's: 
- REST API
- SOAP API
- BULK API
- STREAMING API
 
### REST API
- Based on the RESTful principles
- methods: get, post, put, delete
- support both xml and json
- commonly used for mobile and web apps 

### SOAP API
- robust and powerful web service
- uses wsdl 
- supports only xml
- great for writing server-to-server integrations

### BULK API
- specialized RESTfuul API for loading and querying lots of data at once
- 50000 records or more
- is asynchronous
- example: salesforce dataloader 

### STREAMING API
- listen to changes on an SOQL query
- polling data
