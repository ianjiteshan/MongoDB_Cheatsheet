# MongoDB CHEATSHEET

### View all databases
```sh
show dbs
```

### Create a new or switch databases
```sh
use dbName
```

### View current Database
```sh
db
```

### Delete Database
```sh
db.dropDatabase()
```

# 📁Collection Commands
```sh
Show Collections
show collections
```

#### Create a collection named 'comments'
```sh
db.createCollection('comments')
```

#### Drop a collection named 'comments'
```sh
db.comments.drop()
```

# Row(Document) Commands
```sh
Show all Rows in a Collection 
db.comments.find()
```

#### Show all Rows in a Collection (Prettified)
```sh
db.comments.find().pretty()
```

#### Find the first row matching the object
```sh
db.comments.findOne({name: 'Anjit'})
```

#### Insert One Row
```sh
db.comments.insert({
        'name': 'Anjit',
        'lang': 'JavaScript',
        'member_since': 9
    })
```

#### Insert many Rows
```sh
db.comments.insertMany([{
        'name': 'Anjit',
        'lang': 'JavaScript',
        'member_since': 5
        }, 
        {'name': 'Sohan',
        'lang': 'Python',
        'member_since': 3
        },
        {'name': 'Delisha',
        'lang': 'Java',
        'member_since': 4
    }])
```

#### Search in a MongoDb Database
```sh
db.comments.find({lang:'Python'})
```

#### Limit the number of rows in output
```sh
db.comments.find().limit(2)
```

#### Count the number of rows in the output
```sh
db.comments.find().count()
```

#### Update a row
```sh
db.comments.updateOne({name: 'Shubham'},
{$set: {'name': 'Anjit',
    'lang': 'JavaScript',
    'member_since': 51
}}, {upsert: true})
```

#### Mongodb Increment Operator
```sh
db.comments.update({name: 'Sohan'},
    {$inc:{
        member_since: 2
    }})
```

#### Mongodb Rename Operator
```sh
db.comments.update({name: 'Sohan'},
{$rename:{
    member_since: 'member'
}})
```

#### Delete Row
```sh
db.comments.remove({name: 'Harry'})
```

#### Less than/Greater than/ Less than or Eq/Greater than or Eq
```sh
db.comments.find({member_since: {$lt: 90}})
db.comments.find({member_since: {$lte: 90}})
db.comments.find({member_since: {$gt: 90}})
db.comments.find({member_since: {$gte: 90}})
```
#### Projection (Select Specific Fields)
```sh

db.comments.find({}, {name: 1, lang: 1, _id: 0})
```
#### Sort the Output 
```sh

db.comments.find().sort({member_since: -1})   # Descending
db.comments.find().sort({member_since: 1})    # Ascending
```
#### Indexes
```sh

#Create an index on 'name'
db.comments.createIndex({name: 1})

#View all indexes
db.comments.getIndexes()

#Drop index
db.comments.dropIndex({name: 1})
```
#### Aggregation Example
```sh

db.comments.aggregate([
  {$match: {lang: "Python"}},
  {$group: {_id: "$lang", total: {$sum: 1}}}
])
```
#### Query Operators
```sh

# Not equal
db.comments.find({lang: {$ne: "JavaScript"}})

# In / Not In
db.comments.find({lang: {$in: ["Python", "Java"]}})
db.comments.find({lang: {$nin: ["Python", "Java"]}})
```
#### Logical Operators
```sh

# OR condition
db.comments.find({$or: [{lang: "Python"}, {member_since: {$gt: 4}}]})

# AND condition
db.comments.find({$and: [{lang: "Python"}, {member_since: 3}]})

# NOT condition
db.comments.find({member_since: {$not: {$gt: 3}}})
```
####Array Queries
```sh

# Match exact array
db.comments.insert({name: "Anjit", skills: ["JS", "Mongo", "Node"]})
db.comments.find({skills: ["JS", "Mongo", "Node"]})

# Match if array contains an element
db.comments.find({skills: "Mongo"})

# Match if array contains all specified elements
db.comments.find({skills: {$all: ["Mongo", "Node"]}})

# Match specific array index
db.comments.find({"skills.0": "JS"})

# Match by array length
db.comments.find({skills: {$size: 3}})
```
#### 🛡️Data Validation with Schema
```sh

db.createCollection("users", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["name", "email"],
      properties: {
        name: {bsonType: "string"},
        email: {bsonType: "string"},
        age: {bsonType: "int", minimum: 18}
      }
    }
  }
})
```
#### 🔗Lookup (Manual Join using $lookup)
```sh

db.orders.aggregate([
  {
    $lookup: {
      from: "users",
      localField: "userId",
      foreignField: "_id",
      as: "userDetails"
    }
  }
])
```
####Import and Export Data
```sh

# Export collection to JSON file
mongoexport --db=mydb --collection=comments --out=comments.json

# Import JSON file into collection
mongoimport --db=mydb --collection=comments --file=comments.json --jsonArray

```



# 🛠️Mongoose Basics (Node.js ODM)
#### Install mongoose
```sh
npm install mongoose
```
#### Connect to MongoDB
```sh
const mongoose = require('mongoose')
mongoose.connect('mongodb://localhost:27017/mydb')
```
#### Create Schema
```sh
const commentSchema = new mongoose.Schema({
  name: String,
  lang: String,
  member_since: Number
})
```
#### Create Model
```sh
const Comment = mongoose.model('Comment', commentSchema)
```
#### Insert Document
```sh
const comment = new Comment({name: "Anjit", lang: "JS", member_since: 5})
comment.save()
```





