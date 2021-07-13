---
layout: post
title: "Considerations of Choosing a Primary Database"
date: 2021-07-12 23:50:00 -0400
categories: sdc
---
### The Issue of Scaling

In the perfect world where I was developing Atelier for a company with limitless resources, the prospect of scaling a Postgres database would be as simple as adding more vertical capacity and power. It seems that historically, RDBMS favored as little replication as possible, and thus SQL servers in general still tend to function best when they are big and powerful. On the other hand, MongoDB natively supports database sharding, and scales easily horizontally, able to utilize many, less powerful machines; which are more likely to be the type able to be utilized by the project at hand.

### No Legacy SQL Systems In Place

A major selling point of Postgres and other powerful RDBMS, is that they are very adaptable to infrastructure that already has legacy SQL support. Given that this project has been developed from scratch, there is no necessity to embody SQL.

### Schema Flexibility

Fundamentally, without the use of other magic, a Postgres database needs to be pulled out of commission while schema is being updated. For a retail website, any second of downtime represents dollars lost. In addition, the landscape of retail changes rapidly, and the needs of a retail website continue to evolve over time. It can be readily assumed that frequent schema updates would be necessary to continue to iterate on retail functionality.

### ACID Compliance

The products API is not directly performing transactional functions. Data validity as it pertains to SKU quantities is helpful, but in worst case scenario when an enduser orders more product than actually available, a simple refund response should be able to handle any errors. The predominant opinion around ACID and Mongo is that Mongo is indeed technically ACID compliant, and can be used as such with perhaps the caveat that only one document can be ACID compliant at a time. For use where ACID is critical, most decide on a historically trusted RDBMS for compliance.
 
## The Winner: MongoDB