
### ![](<https://i.imgur.com/2tou8Jo.jpg>)  ###


###  ###


### Update One  ###

 Will update only first encountered document that match the selector:

`
   db.movieDetails.updateOne( {"title": "The Martian"}, {$set: {"poster": "https://images-na.ssl-images-amazon.com/images/M/MV5BMTc2MTQ3MDA1Nl5BMl5BanBnXkFtZTgwODA3OTI4NjE@._V1_UX182_CR0,0,182,268_AL_.jpg"}} )
  `

in this case selector is title "The Martian". In general case "_id" selector would be more appropriate.

If there is existing value for `
   "poster"
  ` field, it will be replaced with new one.

Response should look like that:

`
   { "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
  `

If our query will match more we'll get that count in `
   "matchedCount"
  ` field of the response.


### Increment  ###

 `
   db.movieDetails.updateOne( {"title": "The Martian"}, {$inc: {"tomato.reviews": 3, "tomato.userReviews": 25}} )
  `


### Arrays Updating  ###

 `
  db.movieDetails.updateOne({title: "The Martian"},
 ` `
  {$push: { reviews: { rating: 4.5,
 ` `
  date: ISODate("2016-01-12T09:00:00Z"),
 ` `
  reviewer: "Spencer H.",
 ` `
  text: "The Martian could have been a sad drama film, instead it was a hilarious film with a little bit of drama added to it. The Martian is what everybody wants from a space adventure. Ridley Scott can still make great movies and this is one of his best."} } })
 `

`
  db.movieDetails.updateOne({title: "The Martian"},
 ` `
  {$push: { reviews:
 ` `
  { $each: [
 ` `
  { rating: 0.5,
 ` `
  date: ISODate("2016-01-12T07:00:00Z"),
 ` `
  reviewer: "Yabo A.",
 ` `
  text: "i believe its ranked high due to its slogan 'Bring him Home' there is nothing in the movie, nothing at all ! Story telling for fiction story !"},
 ` `
  { rating: 5,
 ` `
  date: ISODate("2016-01-12T09:00:00Z"),
 ` `
  reviewer: "Kristina Z.",
 ` `
  text: "This is a masterpiece. The ending is quite different from the book - the movie provides a resolution whilst a book doesn't."},
 ` `
  { rating: 2.5,
 ` `
  date: ISODate("2015-10-26T04:00:00Z"),
 ` `
  reviewer: "Matthew Samuel",
 ` `
  text: "There have been better movies made about space, and there are elements of the film that are borderline amateur, such as weak dialogue, an uneven tone, and film cliches."},
 ` `
  { rating: 4.5,
 ` `
  date: ISODate("2015-12-13T03:00:00Z"),
 ` `
  reviewer: "Eugene B",
 ` `
  text: "This novel-adaptation is humorous, intelligent and captivating in all its visual-grandeur. The Martian highlights an impeccable Matt Damon, power-stacked ensemble and Ridley Scott's masterful direction, which is back in full form."},
 ` `
  { rating: 4.5,
 ` `
  date: ISODate("2015-10-22T00:00:00Z"),
 ` `
  reviewer: "Jens S",
 ` `
  text: "A declaration of love for the potato, science and the indestructible will to survive. While it clearly is the Matt Damon show (and he is excellent), the supporting cast may be among the strongest seen on film in the last 10 years. An engaging, exciting, funny and beautifully filmed adventure thriller no one should miss."},
 ` `
 ` `
  { rating: 4.5,
 ` `
  date: ISODate("2016-01-12T09:00:00Z"),
 ` `
  reviewer: "Spencer H.",
 ` `
  text: "The Martian could have been a sad drama film, instead it was a hilarious film with a little bit of drama added to it. The Martian is what everybody wants from a space adventure. Ridley Scott can still make great movies and this is one of his best."} ] } } } )
 ` where `
   $each
  ` modifier pushes every element of provided list in the same "reviews" array in the document.

`
  db.movieDetails.updateOne({ title: "The Martian" },
 ` `
  {$push: { reviews:
 ` `
  { $each: [
 ` `
  { rating: 0.5,
 ` `
  date: ISODate("2016-01-13T07:00:00Z"),
 ` `
  reviewer: "Shannon B.",
 ` `
  text: "Enjoyed watching with my kids!" } ],
 ` `
  $position: 0,
 ` `
  $slice: 5 } } } )
 ` `
   $slice
  ` Modifies the operator to limit the size of updated arrays. Which means that during pushing old elements of a target array will be pushed out from the array and replaced with new one, and in according to value of `
   $slice
  ` modifier array will contain only 5 elements. Has to be used with `
   $each
  ` Depending on "5" or "-5" is used first or last 5 elements will be keep.

`
   $position
  ` modifier set to 0 to be ensure that new elements will be added to the beginning of an array.


### Update Many  ###

 `
   db.movieDetails.updateMany( { rated: null }, { $unset: { rated: "" } } )
  `

`
   $unset
  ` operator will delete "rated" field completely and `
   ""
  ` means that value of the "rated" field doesn't matter.


### Upserts  ###

 Are operations for witch if no document is found matching a filter, we insert a new document.

`
   db.movieDetails.updateOne(
  `  `
   {"imdb.id": detail.imdb.id},
  `  `
   {$set: detail},
  `  `
   {upsert: true});
  `


### Replace One  ###

 `
   db.movies.replaceOne(
  `  `
   {"imdb": detail.imdb.id},
  `  `
   detail);
  `

