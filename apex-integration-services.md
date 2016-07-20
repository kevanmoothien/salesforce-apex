# Apex integration services

##  Overview
Allow you to make call to external webservice or sends http request.
- SOAP - use XML and need to integrate a wsdl document  
- HTTP - use of REST and response as json 

## Prerequisite 
All call to external link must be added to the remote site settings to tell salesforce that it is safe to call these url.

### SOAP Web services
To be able to call a soap webservice in apex, the wsdl must be converted into apex code. 

### HTTP Request
No wsdl or additional file needed to make these types of request. Apex already provide methods to make these call.

