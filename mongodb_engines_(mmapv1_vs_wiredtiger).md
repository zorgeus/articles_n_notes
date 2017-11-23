
### ![](<https://i.imgur.com/2tou8Jo.jpg>)  ###


###  ###


### MMAPv1  ###

 Default engine (at a time of versions 3.0 – 3.3)

Hereby starts by using just

`
   mongod
  `

command.


### WiredTiger  ###


###### In MongoDB since 2014  ######

 `
   mongod --storageEngine wiredTiger
  `

Pros:

*   document level concurrency (“lock free”)
*   Compression
*   No in-place updates

Cons:

*   can be slower when writing rather large documents.

Example:

`
   $ killall mongod
  `

`
   $ mkdir WT
  `

`
   $ mongod --dbpath WT --storageEngine wiredTiger
  `

then start mongo shell:

`
   $ mongo
  `

`
   $ db.foo.stats()
  `

