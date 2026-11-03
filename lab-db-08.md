### Trying Basics I

create, insert, delete, update

#### a. start mongodb

use the same environment from the previous labs (mongodb).

```bash
$docker start mongodb
$mongosh --port 27017
```

let's continue ...

#### b. query database

1. (within mongosh) switch to 'db'

   ```sql
   test> use university;
   ```

   output:

   ```sql
   test> db
   test
   test> use university
   switched to db university
   university>
   ```

2. create collection (equivalent to table)

   insert a single ***document*** to ***collection*** (ie. a single row to table)

   read https://www.mongodb.com/docs/manual/core/document/

   ```sql
   university> professor = {name: "C. Park", major: "Engineering"}
   { name: 'C. Park', major: 'Engineering' }
   ```

   use insert method: insertOne(), db.[collection].[command]

   read https://www.mongodb.com/docs/manual/reference/method/db.collection.insert/

   ```
   university> db.instructor.insertOne(professor)
   ```

   use insert multiple method: insertMany()

   ```
   university> students = [ {name: "John A.", age: 21}, {name: "Marry C.", age: 31},
     {name: "Christ B.", age: 41}, ]
     
   university> db.student.insertMany(students)
   ```

3. find (equivalent to select)

   read https://www.mongodb.com/docs/manual/reference/method/db.collection.find/

   db.[collection].find()

   ```
   university> db.student.find({name: "Marry C."})
   ```

   db.[collection].findOne()

   ```
   university> db.student.findOne({name: "Marry C."})
   ```

4. delete (equivalen to delete or drop)

   db.[collection].deleteOne()

   db.[collection].deleteMany() or db.[collection].drop()

   ```
   university> db.student.deleteOne ({name: "John A."})
   university> db.student.find()
   ```

5. update

   db.[collection].updateOne()

   read https://www.mongodb.com/docs/manual/reference/operator/update/

   ```
   university> db.student.updateOne({name: "Marry C."}, {$set: {age: 30}})
   ```

#### c. create your js file: put all your queries together

1. create an empty query file: sample-mongo.js

   ```shell
   $touch sample-mongo.js
   $vim sample-mongo.js
   ```

2. Insert the following and save

   ```sql
   use university
   // query 1
   db.student.find()
   // query 2
   db.student.findOne()
   ```
   
3. execute your queries in terminal

   ```shell
   $mongosh --port 27017 < sample-mongo.js
   ```

   for example, output:

   ```shell
   test> use university
   switched to db university
   university> // query 1
   
   university> db.student.find()
   [
     {
       _id: ObjectId('66b9c903828d34631628e91c'),
       name: 'John A.',
       age: 21
     },
     {
       _id: ObjectId('66b9c903828d34631628e91d'),
       name: 'Marry C.',
       age: 31
     },
     {
       _id: ObjectId('66b9c903828d34631628e91e'),
       name: 'Christ B.',
       age: 41
     }
   ]
   university> // query 2
   
   university> db.student.findOne()
   { _id: ObjectId('66b9c903828d34631628e91c'), name: 'John A.', age: 21 }
   university> % 
   ```

