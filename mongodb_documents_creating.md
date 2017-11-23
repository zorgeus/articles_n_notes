
### ![](<https://i.imgur.com/2tou8Jo.jpg>)  ###


###  ###


### Insert One command:  ###

 `
   db.moviesScratch.insertOne({"title":"film", "year":"1984", "imdb":"tt459032"})
  `

this creates a document in the collection and create a collections if it's not already exists.

The result should be like this:

`
  {
 ` `
  "acknowledged" : true,
 ` `
  "insertedId" : ObjectId("580e48e550224611f35a8e78")
 ` `
  }
 `


### Insert Many:  ###

 `
   db.moviesScratch.insertMany([{}, {}, {}])
  `

where you inserting an array of documents.

Use `
   {"ordered": false}
  ` to insert all documents despite possible errors during inserting, so probable errors wont disrupt the whole process while errors occur:

`
   db.moviesScratch.insertMany([{}, {}, {}], {"ordered": false})
  `

false - without quotes.

Also documents cam be created by "upserts" during updating.

