### Comparison Operator
$eq = Equal to
$ne = Not Equal to
$gt > Greater than
$lt < Less than
$gte >= Greater than or equal to
$lte <= Less than or equal to

### Logic operators
and: Matches all the specified query clauses
or: At least one of the query clauses matched
nor: Failed to match both given clauses
{<operator>: [{<expression1>},{<expression2>}, ...  {<expressionN>}]}
not: Negates the query requirement

### Expressive Query Operator
$expr:$expr allows the use of aggregation expressions within the query language.
{$expr: {<expression>}} 
It allows us to use variables and conditional statements.

### Array Operators
$push: Allows us to add an element to an array. Turn a field into an array field, if it was previously a different type
$size: {<array field>:{"$size":<number>}} returns a cursor with all documents where the specified array field is exactly the given length
$all: {<array field>:{"$all":<array>}} returns a cursor with all documents in which the specified array field contains all the given elements regardless of their order in the array. 

### Projection
db.<collection>.find({<query>},{<projection>})
1-include the field
0-exclude the field

$eleMatch: {<field> : {"$eleMatch": {<field>: <value>}}}
Matches the documents that contain an array field with an at least one element that matches the specified query criteria

### Querying arrays and Sub-documents
MQL uses dot-notation to specify the address of nested elements in a document.
To use dot-notation in arrays specify the position of the element in the array.
db.collection.find({"field1.other field.also a field": "value"})


### Questions
1.How many documents in the sample_training.zips collection have fewer than 1000 people listed in the pop field?

Ans: 8065

db.zips.find({"pop":{"$lt":1000}}).count()


2.What is the difference between the number of people born in 1998 and the number of people born after 1998 in the sample_training.trips collection?

Ans: 6

db.trips.find({"birth year":1998}).count() 12
db.trips.find({"birth year":{"$gt":1998}}).count() 18

3. Using the sample_training.routes collection find out which of the following statements will return all routes that have at least one stop in them?

Ans:

db.routes.find({ "stops": { "$gt": 0 }}).pretty()
db.routes.find({ "stops": { "$ne": 0 }}).pretty()

4.How many businesses in the sample_training.inspections dataset have the inspection result "Out of Business" and belong to the "Home Improvement Contractor - 100" sector?

Ans: 4

{"result":"Out of Business","sector":"Home Improvement Contractor - 100"}

5.How many zips in the sample_training.zips dataset are neither over-populated(1000000) nor under-populated(5000)?

Ans: 11193

db.zips.find({"$nor":[{"pop":{"$gt": 1000000 }},{"pop": { $lt: 5000 }}]}).count()

6. How many companies in the sample_training.companies dataset were either founded in 2004
[and] either have the social category_code [or] web category_code,[or] were founded in the month of October,[and] also either have the social category_code [or] web category_code?

Ans: 149

db.companies.find({
    $and: [
        {
            $or: [{founded_year: 2004}, {founded_month: 10}]
        },
        {
            $or: [{category_code: "web"}, {category_code: "social"}]
        }
    ]
}).count()

7.What are some of the uses for the $ sign in MQL?

Ans:

$ signifies that you are looking at the value of that field rather than the field name.
$ denotes an operator.

8.Which of the following statements will find all the companies that have more employees than the year in which they were founded?

Ans:

db.companies.find(
    { "$expr": { "$lt": [ "$founded_year", "$number_of_employees" ] } }
  ).count()

db.companies.find(
    { "$expr": { "$gt": [ "$number_of_employees", "$founded_year" ]} }
  ).count()

8.How many companies in the sample_training.companies collection have the same permalink as their twitter_username?

Ans: 1299

db.companies.find({"$expr":{"$eq":["$twitter_username", "$permalink"]}}).count()

9.What is the name of the listing in the sample_airbnb.listingsAndReviews dataset that accommodates more than 6 people and has exactly 50 reviews?

Ans: Sunset Beach Lodge Retreat

db.listingsAndReviews.find({ "reviews": { "$size":50 },"accommodates": { "$gt":6 }})

10.Using the sample_airbnb.listingsAndReviews collection find out how many documents have the "property_type" "House", and include "Changing table" as one of the "amenities"?

Ans: 11

db.listingsAndReviews.find({"property_type":"House", "amenities":{$all:["Changing table"]}})

12.Which of the following queries will return all listings that have "Free parking on premises", "Air conditioning", and "Wifi" as part of their amenities, and have at least 2 bedrooms in the sample_airbnb.listingsAndReviews collection?

Ans:

db.listingsAndReviews.find(
  { "amenities":
      { "$all": [ "Free parking on premises", "Wifi", "Air
        conditioning" ] }, "bedrooms": { "$gte":  2 } } ).pretty()

13.How many companies in the sample_training.companies collection have offices in the city of Seattle?

Ans: 117

db.companies.find({"offices":{"$elemMatch":{"city":"Seattle"}}}).count()

14.Which of the following queries will return only the names of companies from the sample_training.companies collection that had exactly 8 funding rounds?

Ans:

db.companies.find({"funding_rounds": {"$size": 8}},
                  {"name": 1, "_id": 0})

15.How many trips in the sample_training.trips collection started at stations that are to the west of the -74 longitude coordinate?

Ans: 1928

db.trips.find({"start station location.coordinates.0":{"$lt":-74}}).count()

16.How many inspections from the sample_training.inspections collection were conducted in the city of NEW YORK?

Ans: 18279

db.inspections.find({"address.city":"NEW YORK"}).count()

17.Which of the following queries will return the names and addresses of all listings from the sample_airbnb.listingsAndReviews collection where the first amenity in the list is "Internet"?

Ans:


db.listingsAndReviews.find({ "amenities.0": "Internet" },
                           { "name": 1, "address": 1 }).pretty()

18.                           