ObjectId: ObjectId() is a function that produces ObjectId values.

#### Inserting New Documents - insert() and errors

Connect Atlas Cluster: mongodb+srv://<username>:<password>@<cluster>.mongodb.net/admin
Navigate to database: use sample_training
Get random document: db.inspections.findOne()
Try to insert the random document

#### Insert multiple documents specifying the _id values, and using the "ordered": false option.
When you set the "ordered=false", the documents with unique ids are inserted along with the one document have common id.

#### Insert multiple documents specifying the _id values, and using the "ordered": true option.
When you set the "ordered=true", if the first id is duplicated simultaneously then only first will be inserted later it stops to insert any document .

#### Inserting new collection to database
if there is no collection in database then we can just create it.

#### Updating the document
updateOne: updateOne updates any one of the document whichever find first matches with query.
updateMany: updateMany updates all the document matches with query. 

#### Update operator
$inc : The $inc operator increments a field by a specified value.We can update many fields at a time
{$inc: { <field1>: <amount1>, <field2>: <amount2>, ...}}
ex: db.zips.updateMany({ "city": "HUDSON" }, {"$inc": { "pop": 10 }})

$set : The $set operator replaces the value of a field with the specified value.
       We can use this to update many fields
{$set: { <field1>: <value1>, ... }}
ex: db.zips.updateOne({"zip": "12534"}, {"$set": {"pop": 17630}})

$push : The $push operator appends a specified value to an array.
{$push: { <field1>: <value1>, ... }}

#### Deleting Documents
db.<collection name>.drop() deletes given collection.
deleteOne(), deleteMany() deletes documents that matches given query.


#### Questions
1. How does the value of _id get assigned to a document?

Ans:
It is automatically generated as an ObjectId type value.
You can select a non ObjectId type value when inserting a new document, as long as this value is unique to this collection

2. Select all true statements from the following list:
       
Ans:
If a document is inserted without a provided _id value, then the _id field and value will be automatically generated for the inserted document before insertion.
MongoDB can store duplicate documents in the same collection, as long as their _id values are different.

3. Which of the following commands will successfully insert 3 new documents into an empty pets collection?

Ans:
db.pets.insert([{ "_id": 1, "pet": "cat" },
                { "_id": 2, "pet": "dog" },
                { "_id": 3, "pet": "fish" },
                { "_id": 3, "pet": "snake" }])


db.pets.insert([{ "pet": "cat" }, { "pet": "dog" },
                { "pet": "fish" }])

db.pets.insert([{ "_id": 1, "pet": "cat" },
                { "_id": 1, "pet": "dog" },
                { "_id": 3, "pet": "fish" },
                { "_id": 4, "pet": "snake" }], { "ordered": false })  

4. MongoDB has a flexible data model, which means that you can have fields that contain documents, or arrays as their values.

Ans: 
{ "_id": 1,
  "pet": "cat",
  "attributes": [ { "coat": "fur",
                    "type": "soft" },
                  { "defense": "claws",
                    "location": "paws",
                    "nickname": "murder mittens" } ],
  "name": "Furball" }

{ "_id": 1,
  "pet": "cat",
  "fur": "soft",
  "claws": "sharp",
  "name": "Furball" }

{ "_id": 1,
  "pet": "cat",
  "attributes": { "coat": "soft fur",
                  "paws": "cute but deadly" },
  "name": "Furball" }

5. Given a pets collection where each document has the following structure and fields:
{
 "_id": ObjectId("5ec414e5e722bb1f65a25451"),
 "pet": "wolf",
 "domestic?": false,
 "diet": "carnivorous",
 "climate": ["polar", "equatorial", "continental", "mountain"]
}

Ans:
db.pets.updateMany({"pet": "cat" },
                   {"$set": { "type": "dangerous",
                               "look": "adorable"}})

db.pets.updateMany({"pet": "cat"},
                   {"$push": {"climate": "continental",
                                "look": "adorable"}})

6. The sample dataset contains a few databases that we will not use in this course. Clean up your Atlas cluster and get rid of all the collections in these databases:

sample_analytics
sample_geospatial
sample_weatherdata
Does removing all collections in a database also remove the database?

Ans: Yes

7. Which of the following commands will delete a collection named villains?
       
Ans: db.villains.drop()


