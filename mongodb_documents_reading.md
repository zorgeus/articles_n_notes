 ![](<https://i.imgur.com/2tou8Jo.jpg>)

The basic operator for reading documents in a database is `
   find()
  ` :

`
   db.movieDetails.find({"rated": "PG-13",  "year": 2009}).pretty()
  `

in this general case two selectors `
   rated
  ` and `
   year
  ` are " **AND ** ed" together, in other words "AND" logic operator is implicitly used.


### Nested documents reaching:  ###

 `
   db.movieDetails.find({"tomato.meter": 100}).count()
  `

We using the "dot notation". The whole sequence should be closed in quotes.


### Embedded array (list) reaching:  ###

 `
   db.movieDetails.find({"writers": ["Ethan Coen", "Joel Coen"]}).count()
  `

this query will look for writers key on the first level of the document that contains an array with both "Ethan Coen" and "Joel Coen" names, and only this two names in this exact order.

This query:

`
   db.movieDetails.find({"actors": "Jeff Bridges"}).count()
  `

will look for documents that have "actors" key and `
   "Jeff Bridges"
  ` as one of elements of the array on any place in this array.

To match a specific element in an array we can use query like this:

`
   db.movieDetails.find({"actors.0": "Jeff Bridges"}).pretty()
  `

"actors.0" string means that query will find only those films where Jeff Bridges appears on the first place in the list (array).


### Projections:  ###

 To retrieve only particular fields from documents we can use projections.

Use projections as a second argument in `
   find
  ` query.

For instance, this query:

`
   db.movieDetails.find({"rated": "PG"}, {title: 1}).pretty()
  `

will return only `
   _id
  ` and `
   title
  ` fields of documents that match PG rating.

To exclude field we can set it to `
   0
  ` :

`
   db.movieDetails.find({"rated": "PG"}, {"title": 1, "_id":0}).pretty()
  `

Another example:

`
   db.movieDetails.find({"rated": "PG"}, {"actors": 0, "writers":0}).pretty()
  `

