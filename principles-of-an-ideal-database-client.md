```
title: Principles of an Ideal Database Client
tags: [database, client, open source, development]
date: January 25, 2015
slug: principles-of-an-ideal-database-client
excerpt: Discussing the principles that go into an ideal database client
author: Matthew Mueller
```

As we've been building [Gittask](https://gittask.com), we've noticed some very leaky abstractions, specifically around our database client. We've had to write some gnarley boilerplate to handle type conversations and rollbacks. That got me thinking a lot about this question:

> What would the ideal database client would look like?

Database clients come in all shapes and sizes. Some are [awful](https://github.com/mongodb/node-mongodb-native) to work with, some are quite [lovely](https://github.com/pebble/yieldb) to work with. Unfortunately, I've found that all database clients fall short in some way or another. Here's what I believe goes into an ideal database client.

## Principles of an Ideal Database Client

In my mind, the ideal database client would have the following characteristics:

- **Lossless Serialization & Deserialization:**

  The data that goes in, should be exactly the same as the data that comes out. If this is not possible, the data should not go in at all.

- **Polyglot Persistence:**

    Your database client should be able to speak to different backend databases.

- **Atomic Transactions across Databases:**

    Your database client should be able to chain together writes across databases and rollback if there's a failure anywhere in the pipeline.

Let's cover each of these principles in more detail:

### Lossless Serialization & Deserialization

As developers, we shouldn't have to worry about how the database is going to muddle with our data. That's a leaky abstraction and extremely error-prone.

Our database or database client should be responsible for keeping these data structures intact. When you save a Date object, you should expect to get a Date object back out.

The serialization and deserialization steps should be separate but consumed by the ideal database client. There may be cases when you do not use this client for data retrieval, but you will still want the data to be cast properly.

I wrote [superjson](https://github.com/lapwinglabs/superjson) as a first attempt at solving this problem for Node.js.

### Polyglot Persistence

The database landscape has grown immensely over the last couple years. Each new database has certain advantages over the others, but all make certain tradeoffs.

In fact, the [CAP theorem](http://en.wikipedia.org/wiki/CAP_theorem) tells us that it's impossible to have the perfect database and we need to choose which tradeoffs are acceptable for our application.

One workaround for these restrains is this idae of [Polygot Persistence](http://martinfowler.com/bliki/PolyglotPersistence.html), which was popularized by [Martin Fowler](https://twitter.com/martinfowler). Polygot Persistence is the idea that you should pick the right database for the right job. This way we have can our cake and eat it too.

---

Our database client should follow suit. Our database client should be able to speak the language of many different databases. In code that might look like this:

```js
Client(mongo(details)).put(key, value);
Client(redis(details)).get(key);
```

### Atomic Transactions

You should be able to define atomic transactions across databases.
Many databases have a way to do atomic transactions for their respective database, but this is not good enough in the world of polyglot persistence.

For example, when you create a user, usually you perform (at least) two database writes:

    1. Save the user data
    2. Save the user session (so they're logged in)

You could use the same database for both, but then you are sacrificing on either performance or query flexibility. A common polyglot persistence pairing for this task is Mongo for saving user data and Redis for saving the user session.

It's important that these writes are atomic. If one of these writes fails, your application will be in an invalid state, so we need a way for the database client to "rollback" changes if there's an error while running these commands. The simplest way to do this is to take a snapshot and if there's a failure, rollback to the old values. In code this may look something like this:

```js
client.atomic()
    .db(mongo).put('user', obj)
    .db(redis).put('session:' + sid, obj.id)
    .run(fn);
```

Before each write, the ideal client would know how to reverse itself in the event of an error somewhere in the pipeline.

## What about ORM?

I spent a lot of time [using](https://github.com/LearnBoost/mongoose/) and [building](https://github.com/modella/modella) ORMs and I'm now convinced that they are leaky abstraction and do more harm than good as your application grows.

Laurie Voss blogged back in 2011 about why [ORM is an anti-pattern](http://seldo.com/weblog/2011/08/11/orm_is_an_antipattern). This blog post is still very relevant.

I think you can get most of the advantages an ORM offers by having a good standalone data validation library (I recommend [rube](github.com/lapwinglabs/rube)) and our ideal database client.

Then you can write your own models in a performant way with the best tools for the job.

## What about the unique features that databases provide?

You may be wondering: well, some databases have more features than others. How do you write a client that can still take advantage of a database's unique features?

I believe all databases operations can be boiled down to a few low-level operations:

```
- client.get(key)
- client.put(key, value)
- client.del(key)
- client.select(collection) (or "database", "table", "sublevel", etc.)
- client.atomic() (initialize an atomic transaction)
```

I think the ideal database would natively support these operations, but be extended for a database's unique features (via signals). In code it may look something like this:

```js
var client = Client(mongo(details));
client.put(key, value);
client.mongo.query(query)
```

The reason for `client.mongo` is that it's now very easy to locate those features which are unique to mongo. This makes your databases more interchangeable. So if we change our underlying database to LevelDB, our mongo-specific queries will throw:

```js
var client = Client(leveldb(details));
client.put(key, value);
client.mongo.query(query);
```

## Let's build this together

The ideal database has not been built yet. Right now, it's a series of high-level ideas that should go into building this client. I'm calling on the community to help get involved to hammer out the details and harden the implementation.

If you are interested in getting involved or following along, I've set up a repository on [Github](https://github.com/lapwinglabs/yurt), with the working title Yurt.

As always, if you like the work we do or want to follow us along as we learn, follow us on Twitter at [@lapwinglabs](https://twitter.com/lapwinglabs).
