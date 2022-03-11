#### Importing data

mongodump: Allows to get the data in BSON format.
mongodump --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies"

mongoexport: Stores the data in JSON format.
mongoexport --uri="mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --collection=sales --out=sales.json

#### Exporting data

mongorestore: Loads the data from binary database dump created by mongo dump.
mongorestore --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies"  --drop dump

mongoimport : Loads the data from JSON.
mongoimport --uri="mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --drop sales.json

Namespace: It's a concatenation of Database name and collection name.

When we looked at the sample_training.zips collection and issued the following queries as:
{"state": "NY"}
{"state": "NY", "city": "ALBANY"} 

##### Connect to the database go through sample training, collections and find state where state==NY 
mongodb+srv://<username>:<password>@<cluster>.mongodb.net/admin
username: provide your username
password: provide you password
cluster: provide cluster name which you are trying to connect

show dbs
use sample_training
show collections
db.zips.find({"state": "NY"})

db.zips.find({"state": "NY"}).count() # How many state==NY 
db.zips.find({"state": "NY", "city": "ALBANY"})# How many state==NY and city==ALBANY 
db.zips.find({"state": "NY", "city": "ALBANY"}).pretty() # Display in proper format

#### Questions
1. Which of the following documents is correct JSON?
Ans:   
{"name" : "Devi", "age": 19, "major": "Computer Science"}

2. Write BSON or JSON in the numbered blanks in the following sentences to make them true?
Ans:   
MongoDB stores data in BSON and you can then view it in JSON.
BSON is faster to parse and lighter to store than JSON.
JSON Supports fewer type than BSON.

3. Which of the following commands will add a collection that is stored in animals.json to an Atlas cluster?
Ans: mongoimport 

4. In the sample_training.trips collection a person with birth year 1961 took a
trip that started at "Howard St & Centre St". What was the end station name for
that trip?
Ans:
db.trips.find({"birth year":1961,"start station name":"Howard St & Centre St" })

5. Using the sample_training.inspections collection find out how many inspections
were conducted on Feb 20 2015.
Ans:
db.inspections.find({"date":"Feb 20 2015"}).count()

6.Query the zips collection from the sample_training database to find all documents where the state is NY.Iterate through the query results.Find out how many ZIP codes there are in NY state.What about the ZIP codes that are in NY but also in the city of ALBANY?
Ans:
use sample_training

db.zips.find({"state":"NY"})
db.zips.count()
db.zips.find({"state":"NY","city":"ALBANY"}).pretty()   
