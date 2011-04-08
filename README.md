# A Generic Javascript Database

Inspired from TaffyDB, which I never got to work, this mimics the syntax
generally while incorporating chaining to resolve some of the ambiguities
I found in Taffy.

Also, there is a slightly different syntax for conditionals just so
there is no value collisions ( or at least as little as possible )

# It's basically SQL, but for the browser.

Lets start by doing an SQL query and show you how you can do it here:

`select firstname, age from people where age > 30 order by age desc`

Pretty straightforward, right?  In this library, you have to rearrange
your thinking a little bit, but not by much.  We can get the same ends
by doing the following:

`people.find(db('age', '> 30')).order('age', 'desc').select('firstname','age')`

The proper way to think about this is

1. You first filter your search results to the entries you 
are interested in. You can do this in a chained way, such as this:

`db.find({key: value, key1: value}).find({key: db('!== undefined')})`

2. Now you have a subset of data that you can work with.  You can run
a number of familiar commands on it.  for instance:

## db.remove()
This will remove the entries from the database but also return them if
you want to manipulate them.

## db.select(fieldlist)
This will extract the values of a particular key from the filtered list
and then return it as an array or an array of arrays, depending on
which is relevant for the query

You can also do an update

## db.update(fields)
In regular SQL you may find yourself doing something like this:

`update employees set fired = true where tardydays > 40`

Here's how you pull that off here:

`employees.find(db('tardydays', '> 40')).update({fired: true})`

See again, how you do the noun first, that is, describe the data you
want to be working on, and then describe the operations that you want
to do to them.

# Further Examples
`remove from users where lastlogin = false`
becomes:

`users.find({lastlogin: false}).remove();`

# Caveats
There's no notion of joining although it probably wouldn't be that hard