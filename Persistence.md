# Introduction

When building an application with Seaside, one often has a need to persist data. There are a lot of different technological solutions for this problem. This document should help you decide what is the right solution for you.

# Overview

The number of different possible solutions is large:

- do nothing, simply save the image
- write binary files with objects
- write SIXX, smalltalk interchange xml
- use imagesegments
- use a prevayler-like solution (SandstoneDB, sPrevayler)
- use an object-oriented database (OODB) (Gemstone, Goods, Magma, Omnibase)
- use an object-relational mapper (ORM) to a relational database (RDB) (SqueakDBX, ROE, GLORP)
- store data in the cloud (Cloudfork)
- use a service oriented architecture (SOA) where Seaside is only the presentation layer

## Availability

Seaside is available on a number of different platforms. Not all persistence solutions are available on all platforms. It is useful to make a breakdown into:

- operating system/version (Windows, Mac OS X, Linux, etc.)
- smalltalk platform (Squeak, Pharo, Gemstone, VW, VA, Dolphin, GST etc.)
- versions supported

## Special features

Clustering/automatic fail-over etc.

## Scenarios

In different situations, there are different storage needs

1. You are writing a small demonstration program to show your customers, and want to populate the system with some representative data. Add a class instance variable to store the instances, and simply save the image.
1. You have a small system with a few hundred/thousand objects, and are not dependent on external systems. A prevayler-like system like SandstoneDB might be a perfect fit. Each object save means a disk access, so scaling ends with disk speed. A few old versions of the data are kept around, so backing up or reverting is easy. If you want a readable representation, SIXX might help.
1. You have a legacy (relational) database, with extensive reporting written for it. Use an ORM.
1. You have a complex and large object model that has to support changing the object model while developing. The solution is an OODB. Gemstone is the large and proven commercial offering. It has a free version for smaller databases (4GB data, 1 core, 1G ram), and has proven scalability to 500 machines. Magma is an open source OODB, seeing active development and growing more and more advanced functionality.

# Solutions

## Save the image

As smalltalk is image based, simply saving the image will keep the objects around for the next time the image is started. This is very suitable for simple demo’s. Images are often large and take a bit of time to write, which can be ok in a single-user scenario.

## Write binary files with objects

Ramon Leon [writes](http://onsmalltalk.com/simple-image-based-persistence-in-squeak) on how to use a ReferenceStream to save only the part of the image that contains interesting objects in Squeak/Pharo. A similar technique could be done in VA Smalltalk using the ObjectSwapper and in VW using BOSS.

## GemStone

GemStone/S 64 Bit combines a (multi-user/multi-machine) Smalltalk environment with built-in transactional persistence over an object space that can be terabytes in size. When using Seaside in GemStone, the framework automatically "saves the image" as part of serving each page. Thus, the persistence model is to simply save objects in a class variable or a class instance variable as described in the sample code below.

## SIXX

SIXX is the smalltalk instance exchange in xml, allowing smalltalk objects to be exchanged between different smalltalks. SIXX is available for Squeak, Dolphin, VW, Gemstone.

## SandstoneDB

SandstoneDb is a lightweight Prevayler style embedded object database with an ActiveRecord API that doesn’t require a command pattern and works for small apps that a single Squeak image can handle. The idea is to make a Squeak image durable and crash proof and suitable for use in small office applications.

Data is kept in ram for speed and on disk for safety. All data is reloaded from disk on image startup.

Since we’re dealing with live objects in memory, concurrency is handled via optional record level critical sections rather than optimistic locking and commit failures. It’s up to the developer to use critical sections at the appropriate points by using the critical method on the record.

Saves are atomic for an ActiveRecord and all its non ActiveRecord children, for example, an order and its items. There is no atomic save across multiple ActiveRecords. A record is a cluster of objects that are stored in a single file together.

## SandstoneDB GOODS adapter

Nico Schwartz [wrote](https://smalltalkthoughts.blogspot.com/2009/05/sandstonegoods.html) an adapter to use the GOODS OODB instead of the file system. He writes: I can execute 1000 small commits in 30 seconds:

```smalltalk
[ 1000 timesRepeat: [ SDChildMock new save] ] timeToRun.
```
I can execute one commit of 1000 small objects in 3 seconds:

```smalltalk
[ SDActiveRecord commit: [ 1000 timesRepeat: [ SDChildMock new save ] ] ] timeToRun. 
```
Read speed is excellent: reading 5000 objects from the db can be done in 1 second:

```smalltalk
[ SDChildMock findAll ] timeToRun.
```
and once they’re in cache, they are read in 0.1 seconds.

## CouchDB

Göran Krampe wrote: There is a Curl-plugin based Couch API on squeaksource (SS) that I have played with a bit. I have also written a so called "view server" in Squeak for CouchDB which means you can write map/reduce-functions to execute "in the server" in Squeak instead of in javascript. Mostly for fun - not really super useful! :)

At my current customer I have written a C# API for CouchDB that we will release under some BSD license any day now - I will probably take that experience/design and translate it into improvements to the Couch API on SS. I am unsure if the Curl plugin is better/faster than a Squeak-only HTTP client approach.

CouchDB itself is a refreshing experience, vibrant very helpful community, written in Erlang, HTTP API, JSON used as "object format", interesting map/reduce-pattern etc etc. Built for "crazy scaling through replication". Our first tests at my customer reveals great speed and very nice flexibility.

## TokyoTyrant

Göran Krampe wrote: TokyoTyrant is the networking server part used on top of Tokyo Cabinet - a very fast key/value-store written in C. I implemented the TokyoTyrant binary socket protocol recently and [blogged](http://goran.krampe.se/2013/07/16/rethinkdb-yet-another-nosql/) about it.

Awesome speed, very interesting functionality - especially the table db part. I am using (or will be using) both of these from Squeak in different projects.

## GLORP

The GLORP website says: GLORP (Generic Lightweight Object-Relational Persistence) is a simple (well, relatively simple) framework for reading and writing Smalltalk objects from relational databases. Many of its design principles are inspired by The Object People’s TOPLink products, and we extend our thanks to The Object People for sponsoring this project and assisting with development. Most of the initial development was done at Camp Smalltalk 2000 in San Diego.

## ROE

ROE is used together with PostgreSQL. Avi Bryant wrote: Inspired somewhat by SchemeQL, I built a library that models the relational algebra in Squeak. The Relational Object Expressions library, or ROE, doesn’t do any mapping, but it does put the world of tables and rows on an equal footing with objects. Relational expressions (ie, queries) are first class objects, which can be easily passed around, composed, and introsopected. They respond to the Collection protocol, which means that from the outside they look and feel like an ordered collection of Tuple objects, but they’re lazy - composing them and filtering them is free, it’s only once you start iterating over the data that a single SQL query is built and sent out to the database.

## Magma

Chris Muller wrote: Magma is a scalable, fault-tolerant ODBMS for the Squeak platform with highly-transparent access. It may be the only option with high-availability built-in. Uniquely, Magma includes robust large collection support with RDBMS-like query power.

A pure object system, Magma requires no external plugins or modules, therefore applications can be deployed to any machine without regard for any additional existing installed software libraries (just the Squeak VM).

## SqueakDBX

Mariano Martinez Peck wrote on squeak-dev: For those who don’t know what this is about, the aim of this project is to build an [OpenDBX](https://www.linuxnetworks.de/doc/index.php/OpenDBX) wrapper which will allow users to perform relational database operations (DDL, DML and SQL) through a truly open source library. Through this feature, the squeak community will hopefully be able to interact with major database engines, such as Oracle and SQL Server, besides those which are open source, like PostgreSQL, MySQL or Sqlite. Moreover, by integrating this with [GLORP](https://sites.google.com/site/glorpsite/), will allow us to generate a complete and open source solution to relational data base access.

## SOA

It is possible to use Seaside as a front-end for a SOA. Ramon Leon writes: As for the web services, I did it and use SoapCore to bind my Seaside front end to .net web services, but it’s been painful and in all honestly I wouldn’t recommend it, I did it out of necessity not want.

## MySQL-driver

Native driver (for Squeak) for connecting to MySql via TCP/IP

## Magritte-RDB

Maps magritte described objects to SQL tables, and queries into objects, currently only supports the MySQL driver, but OpenDBX support is planned. Magritte provides additional coercions for described data types. One to one associations are handled being retrieved with a Join.

## Cloudfork-AWS

Cloudfork AWS is an open source project that provides easy access from Smalltalk to the Amazon Web Services. One of these services is SimpleDB, a powerful and extremely scalable database engine with a simple REST based API. It is possible to use the API directly but it is also possible to use a higer level API. Cloudfork-ActiveItem provides an ActiveRecord like interface to SimpleDB and Cloudfork-SimpleDB-Magritte makes it easy to store magritte described objects in SimpleDB.

## VOSS

The VOSS open source virtual object storage system extends Smalltalk with integrated database management, providing transparent multi-user access and transaction processing of persistent, versioned, Smalltalk objects directly accessible by normal programming, with efficient persistent Btree collection classes, including the multi-key/multi-value/key-set VirtualDictionarySet for aggregation and query-building. VOSS requires VA Smalltalk and runs on Windows 2000, XP, or Vista.

## VisualWorks

James Robertson wrote: For VW, the preferred solution is Glorp, possibly using the ActiveRecord additions we’ve added for Web Velocity (which is built on top of Seaside). [Glorp](https://sites.google.com/site/glorpsite/) is an open source Object Relational mapping framework for Smalltalk - it’s portable across VisualWorks, Squeak, and VA Smalltalk (in the latter case, Instantiations has said that they intend to make sure it works in an upcoming release). It may be available for other Smalltalk implementations as well, but I’m less familiar with dialects beyond the three I mentioned. Glorp is conceptually related to the old Toplink Smalltalk ORM, but is a completely new implementation, originally created by Alan Knight.

## GNU Smalltalk

Paolo Bonzini wrote: GNU Smalltalk has a database interface called DBI and supporting MySQL, PostgresSQL and SQLite. ROE is also available and integrated with all three backends.

There is also support for Glorp, but it is not the latest version — no ActiveRecord, for example — and it only works with MySQL.

# Sample code

## Keep data in the image

On the class side, add an instance variable:

```smalltalk
ADPerson class 
    instanceVariableNames: 'persons'
```

Add an accessor

```smalltalk
ADPerson class>>persons 
    ^ persons ifNil: [ persons := IdentitySet new ]
```

Make it possible to empty the dataset

```smalltalk
ADPerson class>>reset 
    persons := nil
```

Create a new entity and add it to the dataset

```smalltalk
ADPerson class>>createAndAdd
    | person |
    person := ADPerson new.
    person lastName: 'Jansen'.
    ADPerson persons add: person
```

## Magritte-RDB.

```smalltalk
WSPersonalData realizeAll.
WSPersonalData newRecord name: 'bob'; storeOnDB.
{ a. b. c. } storeOnDB.
```

# Links

- [Gemstone](http://seaside.gemtalksystems.com)
- [SandstoneDB SandstoneDB source](http://www.squeaksource.com/SandstoneDb.html)
- [Goods Goods backend for SandstoneDB Blog on backend](http://www.squeaksource.com/SDGoodsStore.html)
- [Magma Magma mailing list](http://lists.squeakfoundation.org/pipermail/magma/)
- [SIXX](http://www.mars.dti.ne.jp/~umejava/smalltalk/sixx/index.html)
- [Tokyo Cabinet: a modern DBM](http://fallabs.com/kyotocabinet/)
- [GLORP](https://sites.google.com/site/glorpsite/)
- [SqueakDBX](https://wiki.squeak.org/squeak/6052)
- [SOAPCore](http://www.mars.dti.ne.jp/~umejava/smalltalk/soapOpera/soapCore.html)
- [MySQL driver](http://www.squeaksource.com/MySQL.html)
- [Magrite-RDB](http://source.lukas-renggli.ch/magritteaddons/)
- [Cloudfork SimpleDB Cloudfork blog](http://www.squeaksource.com/Cloudfork.html)
- [VOSS](http://voss.logicarts.com)
- [An older site on persistence (and other) issues with squeak](https://www.visoracle.com/squeak/faq/persistency_.html)
- [The squeak wiki also has a page on persistence](https://wiki.squeak.org/squeak/512)