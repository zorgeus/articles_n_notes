 ![](<http://i.imgur.com/2tou8Jo.jpg>)

So if we want to group all our products by manufacturer and show how many there is products of each manufacturer we can use:

`
  db.products.aggregate( [
 ` `
  { $group: { _id: "$manufacturer",
 ` `
  num_products: { $sum: 1 }
 ` `
  }
 ` `
  }
 ` `
  ] )
 ` where `
   "$manufacturer"
  ` is the field by witch we are grouping

`
   $sum: 1
  ` tells to increment the counter by one each time we we find a same manufacturer.

In result:

![](<http://i.imgur.com/19mxvEC.png>)


### Aggregation Pipeline  ###

 `
   $project
  ` – reshape of the documents

Can :

*   remove keys
*   add new keys
*   reshape keys
*   functions that can be used on keys:
`
   $toUpper
  ` – change value to upper case (useful to standardization of values for future grouping)

`
   $toLower
  ` – change value to lower case (useful to standardization of values for future grouping)

`
   $add
  ` – adding something to a value

`
   $multiply
  ` – multiplying a value by a number

Example:

`
  db.products.aggregate([
 ` `
  {$project:
 ` `
  {
 ` `
  _id:0,
 ` `
  'maker': {$toLower:"$manufacturer"},
 ` `
  'details': {'category': "$category",
 ` `
  'price' : {"$multiply":["$price",10]}
 ` `
  },
 ` `
  'item':'$name'
 ` `
  }
 ` `
  }
 ` `
  ])
 ` where “_id: 0” means that we don’t want to see id field in our projection (just like in usual projections). This reshaping won’t change actual db documents, we just get them from a collection in a new “reshaped” form.

If you don’t mention a key, it is not included, except for _id, which must be explicitly suppressed. If you want to include a key exactly as it is named in the source document, you just write key:1, where key is the name of the key.

Result:

![](<https://i.imgur.com/bTsIsCv.png>)

`
   $match
  ` – performs a filtering

Example:

`
  db.zips.aggregate([
 ` `
  {$match:
 ` `
  {
 ` `
  state:"NY"
 ` `
  }
 ` `
  }
 ` `
  ])
 ` next we can group:

`
  db.zips.aggregate([
 ` `
  {$match:
 ` `
  {
 ` `
  state:"NY"
 ` `
  }
 ` `
  },
 ` `
  {$group:
 ` `
  {
 ` `
  _id: "$city",
 ` `
  population: {$sum:"$pop"},
 ` `
  zip_codes: {$addToSet: "$_id"}
 ` `
  }
 ` `
  }
 ` `
  ])
 ` and reshape to make prettier :

`
  db.zips.aggregate([
 ` `
  {$match:
 ` `
  {
 ` `
  state:"NY"
 ` `
  }
 ` `
  },
 ` `
  {$group:
 ` `
  {
 ` `
  _id: "$city",
 ` `
  population: {$sum:"$pop"},
 ` `
  zip_codes: {$addToSet: "$_id"}
 ` `
  }
 ` `
  },
 ` `
  {$project:
 ` `
  {
 ` `
  _id: 0,
 ` `
  city: "$_id",
 ` `
  population: 1,
 ` `
  zip_codes:1
 ` `
  }
 ` `
  }
 ` `
  ])
 ` `
   $text
  ` – performs match by full text search. Full text search should be the first element in a pipeline.

`
  db.sentences.aggregate([
 ` `
  {$match:
 ` `
  {$text: {$search: "tree rat"}}
 ` `
  },
 ` `
  {$project:
 ` `
  {words: 1, _id: 0}
 ` `
  }
 ` `
  ])
 ` Result:

![](<https://i.imgur.com/M5VFpQH.png>)

Or to sort by a higher match order:

`
  db.sentences.aggregate([
 ` `
  {$match:
 ` `
  {$text: {$search: "tree rat"}}
 ` `
  },
{$sort:
    {score: {$meta: "textScore"}}
},
 ` `
  {$project:
 ` `
  {words: 1, _id: 0}
 ` `
  }
 ` `
  ])
 `

`
   $group
  ` – aggregation

`
   $ sort
  ` – sorting

`
   $skip
  ` – skips

`
   $limit
  ` – limitation

`
   $unwind
  ` – unwinds arrays. Unjoints an array and rejoin it for more simple use

`
  db.items.aggregate([{$unwind:"$attributes"}]);
 ` For instance:

{ a: 1, b: 2, c: [ “apple”, “pear”, “orange” ] }

will be converted to :

{ a: 1, b: 2, c: “apple”}

{ a: 1, b: 2, c: “pear”}

{ a: 1, b: 2, c: “orange”}

Careful. Using of unwind leads to “data explosion”.

`
   $out
  ` – where to output results

Example with compound document id:

`
  db.products.aggregate( [
 ` `
  { $group: { _id: { "manufacturer": "$manufacturer",
                                             "category": "$category"
                                           },
 ` `
  num_products: { $sum: 1 }
 ` `
  }
 ` `
  }
 ` `
  ] )
 ` In result:

![](<https://i.imgur.com/xlYrDvO.png>)


### Group Aggregation Expressions  ###

 `
   $sum
  ` – can count or sum key values

Example

`
  db.products.aggregate( [
 ` `
  { $group: { _id: {"maker": "$manufacturer"},
 ` `
  sum_prices: { $sum: "$price" }
 ` `
  }
 ` `
  }
 ` `
  ] )
 ` which will create _id field with value as a document and sum up all prices from the “price” field for each manufacturer.

result:

![](<https://i.imgur.com/yInMbHc.png>)

`
   $avg
  ` – compute the average of key values

`
  db.products.aggregate( [
 ` `
  { $group: { _id: {"category": "$category"},
 ` `
  avg_price: { $avg: "$price" }
 ` `
  }
 ` `
  }
 ` `
  ] )
 ` Result:

![](<https://i.imgur.com/XAVwn3v.png>)

`
   $min
  ` – return minimum value of keys

`
   $max
  ` – return maximum value of keys

`
  db.products.aggregate( [
 ` `
  { $group: { _id: {"maker": "$manufacturer"},
 ` `
  "maxprice": { $max: "$price" }
 ` `
  }
 ` `
  }
 ` `
  ] )
 ` Result:

[![maxprice_result](<http://163.172.186.144/wp-content/uploads/2016/11/maxprice_result.png>)](<http://163.172.186.144/articles/mongodb-aggregation/maxprice_result/>)

`
   $push
  ` – creates an array, where puts all values for a chosen field. All encountered values will be added to the array, doesn’t check if they are repetitive.

`
  db.products.aggregate( [
 ` `
  { $group: { _id: {"maker": "$manufacturer"},
 ` `
  "categories": { $push: "$category" }
 ` `
  }
 ` `
  }
 ` `
  ] )
 ` Result:

![](<https://i.imgur.com/Hr0QK6H.png>)

`
   $addtoSet
  ` – creates an array, where puts different values for a chosen field, makes sure that each value will be represented only once (no repetition); if value is already there it will be ignored.

`
  db.products.aggregate( [
 ` `
  { $group: { _id: {"maker": "$manufacturer"},
 ` `
  "categories": { $addToSet: "$category" }
 ` `
  }
 ` `
  }
 ` `
  ] )
 ` in this case it will group by manufacturers, and store all encountered categories for any particular manufacturer into “categories” array if it’s not already in it.

Result:

![](<https://i.imgur.com/V2fF5kF.png>)

`
   $first
  ` – return first value after sorting, if there was no sorting the result is arbitrary

`
  db.zips.aggregate([
 ` `
  /* get the population of every city in every state */
 ` `
  {$group:
 ` `
  {
 ` `
  _id: {state:"$state", city:"$city"},
 ` `
  population: {$sum:"$pop"},
 ` `
  }
 ` `
  },
 ` `
  /* sort by state, population */
 ` `
  {$sort:
 ` `
  {"_id.state":1, "population":-1}
 ` `
  },
 ` `
  /* group by state, get the first item in each group */
 ` `
  {$group:
 ` `
  {
 ` `
  _id:"$_id.state",
 ` `
  city: {$first: "$_id.city"},
 ` `
  population: {$first:"$population"}
 ` `
  }
 ` `
  }
 ` `
  ])
 ` Result:

![](<https://i.imgur.com/qh0E94p.png>)

`
   $last
  ` – return last value after sorting, if there was no sorting the result is arbitrary


### Multiple Grouping  ###

 To get an average for each class:

`
  db.grades.aggregate([
 ` `
  {'$group':{_id:{class_id:"$class_id", student_id:"$student_id"}, 'average': {"$avg":"$score"}}},
 ` `
  {'$group':{_id:"$_id.class_id", 'average':{"$avg":"$average"}}}])
 ` Result after the first stage:

![](<https://i.imgur.com/C10fqbI.png>)

Result after the second stage:

![](<https://i.imgur.com/yNdWfFC.png>)

