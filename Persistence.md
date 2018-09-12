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