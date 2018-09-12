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