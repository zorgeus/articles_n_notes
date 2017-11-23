 ![](<https://i.imgur.com/2tou8Jo.jpg>)

General organization:

Database <-- Collections <-- Documents

To import new db from json and store it in a particular collection:

`
   mongoimport --drop -d students -c grades grades.json
  `

where "students" - new db name

"grades" - new collection name

"grades.json" - json file with documents that looks like this:

![](<https://i.imgur.com/6HSszsv.png>)

To add/restore a new db to your list of dbs from dump:

1.  Open a directory with dump
2.  Press and hold SHIFT, then right mouse button click
3.  Click "Open command window here"
4.  Type `
        mongorestore dump
       ` and hit Enter
To show all dbs : `
   show dbs
  `

To change current db : `
   use <dbname>
  ` after that you will refer to this current db as `
   db
  ` . Example:

`
   db.moviesScratch.find()
  `

where db - the current db

moviesScratch - collection in this db

find() - command to find all documents in this collection.

To show all collections : `
   show collections
  `

To find some document in a collection:

`
   db.moviesScratch.find({"title":"movie"}).pretty()
  `

To export data to a db:

`
   mongoexport --db test --collection traffic --out traffic.json
  `

To switch to a new db:

`
   use <dbname>
  `

to import data from file into a new collection in Windows Powershell:

`
   Get-Content .\create_student_collection.js | mongo
  `

