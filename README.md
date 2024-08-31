# MongoDB

# command used in MONGODB:
doskey /history
### CMD
| Command | Description |
| ------- | ----------- |
| `mongosh` | to enter into that |
|`show dbs`|to look all databases|
|`use dbs-name`|to create data-bases also for switch from one dbs to data-base name mentioned|
|`db.dropDatabases()`|to dealet data-base|
|`db.createCollection 'coll-name' `|for creating a collection|
|`show collections`|it will show all collection|
|`db.coll-name.drop()`|to dealet the collection|
|`cls`|stands for clear|


## inserting of data inside the data-bases:
```
db.<collection-name>.insertMany([{"name":"pradhan", "age" : 20},{"name":"gopal", "age":23},{"name":"j-pradhan","age":20}]);
```
## if any data may occur false:
```
db.people.insertMany([
  { "name": "pradhan", "age": 20 },
  { "name": "gopal", "age": 23 },
  { "name": "j-pradhan", "age": 20 }
], { ordered: false });
```
## for insertOne element:
```
db.<collection-name>.insertOne({
  "name": "pradhan",
  "age": 20
});
```
### To find documents in MongoDB, you use the find method. Here's how you can use it in various scenarios:

```
db.<collection-name>.find()
db.<collection-name>.find({ "field": "value" })
db.<collection-name>.find({ "field": "value" }, { "field1": 1, "field2": 1 })
db.<collection-name>.find().limit(n)
db.people.find().limit(5)
db.people.findOne({ "name": "pradhan" })

```
### consider people be the collection:

```
db.people.find({ "age": { $gt: 20 } }, { "name": 1, "age": 1, "_id": 0 })
         .sort({ "name": 1 })
         .limit(5)
```
### import jsonfile in MongoDB:
(this cmd use if the josndile in list format):

[
  { "name": "pradhan", "age": 20 },
  { "name": "gopal", "age": 23 },
  { "name": "j-pradhan", "age": 20 }
]

```
mongoimport --db <database> --collection <collection> --file <path-to-file> --jsonArray

mongoimport --db testdb --collection people --file data.json --jsonArray

```
(if-not -->no comma):

{ "name": "pradhan", "age": 20 }
{ "name": "gopal", "age": 23 }
{ "name": "j-pradhan", "age": 20 }

```
mongoimport --db testdb --collection people --file data.json 

mongoimport --db <database> --collection <collection> --file <path-to-file>
```
# command to show all data:
```
mongoexport -d <database-name> -c <collection-name> -o <file-path>

mongoexport --db <database-name> --collection <collection-name> --out <file-path> [options]

mongoexport --db testdb --collection people --out data.json

```
## comparison operators :
$eq,$ne,$gt,$gte,$lt,$lte
```
db.collectionName.find({'fieldname':{$operator:value}})

db.products.find({'price':{$eq:69}})

db.products.find({'price':{$eq:699}}).count()

db.products.find({'price':{$ne:699}}).count()

db.products.find({'price':{$gt:699}}).count()

db.products.find({'price':{$gte:699}}).count()


```
### $in,$nin (return true or false) :
```
db.collection.find({price:{$in:[249,129,39]}})
db.products.find({'price':{$in:[699,129,69]}})
```

### count(),limit(),skip(),sort()

```

db.products.find({price:{$gt:250}}).limit(5);

db.products.find({price:{$gt:250}}).limit(5).skip(2);

db.products.find({price:{$gt:1250}}.limit(3).sort({price:1}));-->chote-se-bada

db.products.find({price:{$gt:1250}}.limit(3).sort({price:-1}));-->bada-se-chota

db.products.find({'price':{$not: {$eq:100}}}).count();

db.products.find({'price':{$ne:100}}).count()

```

# complex Expression:
{$expr:{operator:[field,value]}}

EX:
```
db.products.find({$expr:{$gt:['$price',1340]}});

```
### multiplication with greaterthan:
```
shop> db.sales.find({$expr:{$gt:[{$multiply:['$quantity','$price']},'$targetPrice']}})
```
## $exits

{field:{$exists:<boolean>}}
```
db.products.find({price:{$exists:true}}).count();

 db.products.find({price:{$exists:true},price:{$gt:1250}});
```
## $type(to check Bson-data type:)

{field:{$type:"<bson-data-type>"}}
```
{field:{$type:"<bson-data-type>"}}

db.products.find({price:{$type:'number'}}).count()

db.products.find({price:{$type:'string'}}).count()

db.products.find({isFeatured:{$type:'bool'}}).count()
```

## $size:
$size:

{field:{$size:<array-length>}}

in our above cases we performed some operation if it is inside array/list otherwise in objectForm but if it contain both:
```
shop> db.comments.find()

[

  {

  _id: 2,
    title: 'Deep Dive into Aggregation Framework',
    content: 'The aggregation framework in MongoDB...',
    author: 'Jane Smith',
    comments: [
      { user: 'Charlie', text: 'Very informative!' },
      { user: 'David', text: 'Well explained.' }
    ],
    metadata: { views: 800, likes: 70 }
  }
  ```
the code gives like the comments who contain 4 OBJECT
```
{field:{$size:<array-length>}}
shop> db.comments.find({'comments':{$size:4}})
[
  {
    _id: 1,
    title: 'Introduction to MongoDB',
    content: 'MongoDB is a popular NoSQL database...',
    author: 'John Doe',
    comments: [
      { user: 'Alice', text: 'Great article!' },
      { user: 'Bob', text: 'Thanks for sharing.' },
      { user: 'Eva', text: 'Its beatifull!' },
      { user: 'jessy' }
    ],
    metadata: { views: 1000, likes: 50 }
  }
]
```
# projection:

```
 {
    _id: 1,
    title: 'Introduction to MongoDB',
    content: 'MongoDB is a popular NoSQL database...',
    author: 'John Doe',
    comments: [
      { user: 'Alice', text: 'Great article!' },
      { user: 'Bob', text: 'Thanks for sharing.' },
      { user: 'Eva', text: 'Its beatifull!' },
      { user: 'jessy' }
    ],
    metadata: { views: 1000, likes: 50 }
  }
  ```
  how to achieve: only this
  ```
  comments: [
      { user: 'Alice', text: 'Great article!' },
      { user: 'Bob', text: 'Thanks for sharing.' },
      { user: 'Eva', text: 'Its beatifull!' },
      { user: 'jessy' }
    ]
```

it just only return comments who contain only 4 value

Rules:

-->to include specific field,use projection with a value of 1 for the field i want


```
shop> db.comments.find({'comments':{$size:2}},{title:1,_id:0})
[
  { title: 'Deep Dive into Aggregation Framework' },
  { title: 'Introduction to MongoDB' },
  { title: 'Optimizing MongoDB Performance' },
  { title: 'Schema Design Best Practices' },
  { title: 'Getting Started with NoSQL Databases' },
  { title: 'Advanced Queries in MongoDB' }
]
//here it says give me the out put where field is comments having size --2(obj) with showing title(1--show) but not id(0--not show) 
```
-->to exclude field use projection {title:0,_id:0} with a value of 0 for the field you want to exclude
```
shop> db.comments.find({'comments':{$size:4}},{title:0,_id:0})
[
  {
    content: 'MongoDB is a popular NoSQL database...',
    author: 'John Doe',
    comments: [
      { user: 'Alice', text: 'Great article!' },
      { user: 'Bob', text: 'Thanks for sharing.' },
      { user: 'Eva', text: 'Its beatifull!' },
      { user: 'jessy' }
    ],
    metadata: { views: 1000, likes: 50 }
  }
]
shop>
```
---> Dont do like: One show one hide
```
shop> db.comments.find({'comments':{$size:4}},{title:1,author:0,_id:0})

it produce error--> _id is an exceptional case 
MongoServerError[Location31254]: Cannot do exclusion on field author in inclusion projection
shop>
```
you can do like:
```
shop> db.comments.find({'comments':{$size:4}},{title:0,author:0,_id:0}) --> dont show

shop> db.comments.find({'comments':{$size:4}},{title:1,author:1,_id:0}) --> show both

_id:0 / _id:1 is an exceptional by default generate
```
# Embedded Documents:
```
From all to only
```
HOW TO ACHIVE :only this
```
shop> db.comments.find({'comments.user':'Eva'})
[
  {
    _id: 1,
    title: 'Introduction to MongoDB',
    content: 'MongoDB is a popular NoSQL database...',
    author: 'John Doe',
    comments: [
      { user: 'Alice', text: 'Great article!' },
      { user: 'Bob', text: 'Thanks for sharing.' },
      { user: 'Eva', text: 'Its beatifull!' },
      { user: 'jessy' }
    ],
    metadata: { views: 1000, likes: 50 }
  },
  {
    _id: 3,
    title: 'Getting Started with NoSQL Databases',
    content: 'NoSQL databases provide flexible data models...',
    author: 'Sarah Williams',
    comments: [
      { user: 'Eva', text: 'Very helpful!' },
      { user: 'Frank', text: 'Clear explanations.' }
    ],
    metadata: { views: 1200, likes: 40 }
  }
]
```
db.collection.find({"parent.child":value})

```
db.comments.find({'comments.user':'Eva'})
```

# Question:
find documents where the views in metadata field >1200.

Ans:
```
shop> db.comments.find({'metadata.views':{$gt:1200}})
[
  {
    _id: 4,
    title: 'Advanced Queries in MongoDB',
    content: 'Learn how to write complex queries...',
    author: 'Michael Johnson',
    comments: [
      { user: 'Grace', text: 'Mind-blowing content!' },
      { user: 'Henry', text: 'Impressive examples.' }
    ],
    metadata: { views: 1500, likes: 60 }
  }
]
```
Q:
we need to find out the documents where the user in comments = henry and also in the metadata like value > 50

```
db.comments.find({'comments.user':'Henry' , 'metadata.likes':{$gt:50}})
```
shop> db.comments.find({'comments.user':'Henry' , 'metadata.likes':{$gt:50}})
[
  {
    _id: 4,
    title: 'Advanced Queries in MongoDB',
    content: 'Learn how to write complex queries...',
    author: 'Michael Johnson',
    comments: [
      { user: 'Grace', text: 'Mind-blowing content!' },
      { user: 'Henry', text: 'Impressive examples.' }
    ],
    metadata: { views: 1500, likes: 60 }
  }
]
```
Q:
we need to return an comments array which must have this two user= alice & vinod only in it
```
```
db.comments.find({'comments.user':{$all:['Alice','Vinod']}})
[
  {
    _id: 7,
    title: 'Introduction to MongoDB',
    content: 'MongoDB is a popular NoSQL database...',
    author: 'Vinod Thapa',
    comments: [
      { user: 'Alice', text: 'Awesome article!' },
      { user: 'Vinod', text: 'Thanks for sharing.' }
    ],
    metadata: { views: 1000, likes: 70 }
  }
]
```
Q:
user = Vinod and text = Thanks for sharing
```
db.documents.find({'comments':{elemMatch:{'user':'Vinod','text':'Thnaks for sharing'}}})
```
