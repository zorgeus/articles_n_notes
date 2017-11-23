 ![](<https://i.imgur.com/2tou8Jo.jpg>)


### Query and Projection Operators  ###

 [Take a look at the documentation](<https://docs.mongodb.com/manual/reference/operator/query/>)


### Comparison Operators  ###

 `
   db.movieDetails.find( { "runtime": { $gt: 90 } } ).pretty()
  `

`
   db.movieDetails.find( { "runtime": { $gt: 90, $lt: 120 } } ).pretty()
  `

`
   db.movieDetails.find( { "runtime": { $gte: 90, $lte: 120 } } ).pretty()
  `

`
  db.movieDetails.find( { "tomato.meter": { $gte: 95}, "runtime":{ $gt: 180 }}, { "title": 1, "runtime": 1, "_id":0} ).pretty()
 `

"Not Equal" operator:

`
   db.movieDetails.find( { "rated": { $ne: "UNRATED" } } ).pretty()
  `

in this case query will return also documents that has no "rated" field at all.

"In" operator:

`
   db.movieDetails.find( { "rated": { $in: ["G", "PG"] } } ).pretty()
  `

this query will return all documents that has G or PG rating. "$in" operator always should have an array as value.


### Element Operators  ###

 Checking existence of a field:

`
   db.movieDetails.find( { "tomato.meter": {$exists : true} } ).pretty()
  `

Checking a type of a value of a field:

`
   db.moviesScratch.find( { "_id": {$type : "string"} } ).count()
  `


### Logical Operators  ###

 OR

`
   db.movieDetails.find( { $or: [{"tomato.meter": {$gt: 95}}, {"metacritic": {$gt: 88}}] } ).pretty()
  `

AND

because, as we said earlier, "AND" operator is implied by default,

it is appropriate to use only when we have to use the same key more than once to check different types of values, specify multiple constraints on the same field.

For instance:

`
   db.movieDetails.find( { $and: [ {"metacritic": {$ne: null} }, {"metacritic": {$exists: true}} ] } ).pretty()
  `


### Regex operators  ###

 `
   db.movieDetails.find( "awards.text": {$regex: /^Won\s.*/} ).pretty()
  `


### Array operators  ###

 `
   $all
  ` operator is demanding field to contain all elements of a passed array (e.g. "Comedy and Crime and Drama"):

`
   db.movieDetails.find( "genres": {$all: ["Comedy", "Crime", "Drama"]} ).pretty()
  `

`
   $size
  ` allows us to match documents based on a length of an array:

`
   db.movieDetails.find( "countries": {$size: 1} ).pretty()
  `

$elemMatch required that all criteria should be satisfied in a single element of an array field:

`
   db.movieDetails.find( "boxoffice": {$elemMatch: {"country": "UK", "revenu": {$gt: 15}} } ).pretty()
  `

in this case `
   {"country": "UK", "revenu": {$gt: 15}}
  ` is the criteria against which we want to match each element of the array.


### Examples from the HW  ###

Find all exam scores greater than or equal to 65, and sort those scores from lowest to highest.

What is the *student_id * of the lowest exam score above 65?

`
   db.grades.find({ "type": "exam", "score": {$gte: 65}}).sort({"score": 1}).limit(1).pretty()
  `

