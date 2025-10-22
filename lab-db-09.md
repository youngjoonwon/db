### Trying basics II

##### a. let's complete more queries.

- Query #1: Complete the following collection data called "courses". The course and professor names are provided below.

  Fill in distinct student names (3 students per course) for all the courses where one student is taking at least two courses of your choice.

  courses = [
      {
        title: "network",
        students: [ "", "", "" ],
        professor: "alice"
      },
      {
        title: "data structure",
        students: [ "", "", "" ],
        professor: "bob"
      },
      {
        title: "ai",
        students: [ "", "", "" ],
        professor: "alice"
      }
    ]

- Query #2: Provide the result of querying "data structure" class information.
  
- Query #3: Provide the result of querying all the courses the professor "alice" is teaching.

- Query #4: What is the student's name who is taking at least two courses? Provide the result of removing him/her from all the courses. Also show the courses information after.

  
##### b. all the queries above in a single js file and execute in terminal.

  ```sql
  use university
  // query 1
  ...
  // query 2
  ...
  ```
  ```shell
  $mongosh --port 27017 < more-queries.js
  ```
##### c. other commands (in mongosh)

```shell
$show dbs
$db.dropDatabse()
```

##### No submission required. 
