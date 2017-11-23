 ![](<https://i.imgur.com/2tou8Jo.jpg>)

With indexes writing is slower (if writing affects indexed fields) and reading is much faster.


### Retrieving an information about query work  ###

 `
   db.students.explain().find()
  `

`
  db.tweets.explain("executionStats").find( { "user.followers_count" : { $gt : 1000 } } ).limit(10).skip(5000).sort( { created_at : 1 } )
 `


### Creating indexes  ###

 `
   db.students.createIndex({student_id:1})
  `

`
   db.students.createIndex({student_id:1, class_id: -1})
  `

where “1” means ascending index and “-1” – descending.


### Discovering and deleting indexes  ###

 `
   db.students.getIndexes()
  `

`
   db.students.dropIndex({student_id:1})
  `

