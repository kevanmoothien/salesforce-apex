# Intro to apex 

## Basic operation
``` java
//variable declaration
Integer val = 0;
string str = 'Super test'; // apex allow only single quote for string variable
boolean isDeleted = true; 

/*Concatenate string*/
string firstname = 'John';
string lastname = 'Doe';
string name = firstname + ' ' +  lastname;
```

## Collection
A collection in apex is either a "List", "Set" or a "Map"

``` java
/**
* List
*/
//Populating method 1
list<string> carList = new list<string>();  //initializing an empty list
/*Populating the list*/
carList.add('Honda');
carList.add('Toyota');
carList.add('Mazda');

//Populating method 2
list<string> carList = new list<string>{'BMW', 'Mercedes', 'Audi'};

/**
* Set
*/
set<string> referenceSet = new set<string>();
referenceSet.add('test');
//or 
set<string> referenceSet = new set<string>{'test'};

/**
* Map
* A map consists of a key and value. The key should always be a primitive type (string, integer, id, ...).
*/
map<string, string> countryCodeMap = new map<string, string>(); //initiliazing an empty map
//Populating a map
countryCodeMap.put('Mauritius', 'MU');
countryCodeMap.put('France', 'FR');
//or
map<string, string> countryCodeMap = new map<string, string>{'Mauritius'=>'MU', 'France'=>'FR'};

//accessing a value for a specific key in the map
string franceCode = countryCodeMap.get('France'); //returns FR
```

## sObject
Any standard and custom object in Salesforce is represented as an sObject in Apex.

Example 

| Object type | Name     | Api name (as represented in apex) |
| ----------- | ---------- | ----------------------------------- |
| Standard | Account  | Account |
| Standard | Contact | Contact |
| Custom | Car | Car__c |
| Custom | Job | Job__c |

``` java
//Create a new instance of an sObject
Contact con1 = new Contact(); //initializing an instance of a contact
con1.Firstname = 'Jane';
con1.Lastname = 'Doe';

// or 
Contact con1 = new Contact(Firstname='Jane', Lastname='Doe');

/*custom object*/
Job__c job = new Job__c();

//creating an instance of an existing record
Account salesFr = new Account(Id='0012400000YwpHM'); //you just need to instantiate with the field "Id"

```

### Note
A lookup or master-detail relationship fields in apex is represented by an Id.

## DML
DML operations consists of: 
- insert
- update
- upsert
- delete
- undelete
- merge

### Insert
```java
Account telecom = new Account(Name='Telecom');
insert telecom;
```
### Update
```java
telecom.Name = 'Telecom - Head office';
update telecom;
```
### Upsert
It does an insert if record not found else update if found.
Generally use with an ExternalId (a flag on a field to make it an index)
```java
//account does not exist
Account sbm = new Account(Name='SBM');
upsert sbm; // will create an account with Name = SBM
//account exist
Account sbm = new Account(Id='0012400000YwpHM', Name='SBM - Closed');
upsert sbm; // will update with Name = 'SBM - Closed'

//update a contact based on its external id (here Email is defined as external id)
Contact con1 = new Contact(Email='plop@test.com', Lastname='Plop');
upsert con1 Contact.Email;
```
### Delete and Undelete
Can only delete instance which contains an Id.
```java
Account sbm = new Account(Id='0012400000YwpHM');
delete sbm;

undelete sbm;
```

## SOQL queries
SOQL query will always return the following: 
- an sobject
- a list of sobjects 
- a list of objects

### Basic query
```java
//Returning only one record
Contact jane = [SELECT Id, Firstname, Lastname FROM Contact WHERE Id = '0032400000GRVvY'];

//returning a list of record
list<Account> accountList = [SELECT Id, Name FROM Account];
```
### Advanced query 
```java
// returning a list of contact with their account name
list<Contact> contactList = [SELECT Id, 
                                    Firstname,
                                    Lastname,
                                    Account.Name
                            FROM Contact
                            WHERE AccountId != null];
  
// returning list of account with their children(contacts)
list<Account> accountList = [SELECT Id, Name,
                                (SELECT Id, Firstname, Lastname from Contacts)
                            FROM Account];
```
Notice that the child relationship is in plural.
If a custom object was used here
- parent relationship: objectName**__r**
- child relationship: objectName**s__r**


## Loops
``` java
for(integer i = 0; i < size; i++){
  // code here
}

/*Example*/
list<string> firstnames = new list<string>{'John', 'Jane'};
list<string> lastnames = new list<string>{'Smith', 'Doe'};
list<string> names= new list<string>();
for(integer i = 0; i < firstnames.size(); i++){
  string name = firstnames[i] + ' ' + lastnames[i];
  names.add(name);
}

//or 
for(type variable: list<type>){
  //do any operation with variable
}

/*Examples*/
integer total = 0;
list<Account> accountList = [SELECT id, NumberOfEmployees FROM Account];
for(Account acc: accountList){
  if (acc.NumberOfEmployees != null){
    total = total + acc.NumberOfEmployees;
  }
}

```

## Debug Apex code
``` java 
string name = 'Super test';
system.debug('### '+name); // ### Super test

```
