Aggregation Framework: Aggregation Framework in its simplest form, another way to query data in MongoDB

$group: An operator that takes the incoming stream of data, and siphons it into multiple distinct reservoirs 

$sort(aggregation): Sorts all input documents and returns them to the pipeline in sorted order.
It has following form:
{$sort: {<field1>: <sort order>, <field2>: <sort order> ...}}

$limit: It limits the number of outputs

### Indexes
In a book - an alphabetical list of names, subjects, etc., with references to the places where they occur, typically found at the end of book.
In a database - special data structure that stores a small portion of the collections data set in an easy to traverse form.

### Data Modeling
Data modeling - a way to organize fields in a document to support your application performance and querying capabilities.Data that is used together should be stored together.Evolving application implies an evolving data model.

### Upsert
Everything in MQL that is used to locate a document in a collection can also be used to modify this document.
db.collection.updateOne({<query to locate>},{<update>})

Upsert is a hybrid of update and insert, it should only be used when it is needed.
db.collecction.updateOne({<query>},{<update>},{"upsert":true})

When upsert is true, update the matched document if there is a match else inserting a new document

### Questions

1.What room types are present in the sample_airbnb.listingsAndReviews collection?

Ans:

db.listingsAndReviews.aggregate([{"$group":{"_id":"$room_type"}}])

[
  { _id: 'Private room' },
  { _id: 'Entire home/apt' },
  { _id: 'Shared room' }
]

2.What are the differences between using aggregate() and find()?

Ans:

aggregate() can do what find() can and more.
aggregate() allows us to compute and reshape data in the cursor.

3.Which of the following commands will return the name and founding year for the 5 oldest companies in the sample_training.companies collection?

Ans:

db.companies.find({ "founded_year": { "$ne": null }},
                  { "name": 1, "founded_year": 1 }
                 ).sort({ "founded_year": 1 }).limit(5)

db.companies.find({ "founded_year": { "$ne": null }},
                  { "name": 1, "founded_year": 1 }
                 ).limit(5).sort({ "founded_year": 1 })

4.In what year was the youngest bike rider from the sample_training.trips collection born?

Ans: 1999

db.trips.find({"birth year":{"$ne":""}}).sort({"birth year":-1}).limit(1)

5.Jameela often queries the sample_training.routes collection by the src_airport field like this:db.routes.find({ "src_airport": "MUC" }).pretty()

Ans:

db.routes.createIndex({ "src_airport": -1 })

6.How does the upsert option work?

Ans:

By default upsert is set to false.
When upsert is set to false and the query predicate returns an empty cursor then there will be no updated documents as a result of this operation.
When upsert is set to true and the query predicate returns an empty cursor, the update operation creates a new document using the directive from the query predicate and the update predicate.