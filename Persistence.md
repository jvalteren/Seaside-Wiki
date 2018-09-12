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