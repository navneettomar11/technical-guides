
# Introduction
MongoDB is a document databse designed for ease of developmenet and scaling. MongoDB offers both a Community and an Enterprise version of the database:
* MongoDB Community is the source available and free to use edition of MongoDB.
* MongoDB Enterprise is available as part of the MongoDB Enterprise Advanced subscription and includes comprehensive support of your MongoDB deployment. MongoDB Enterprise also adds enterprise-focused features such as LDAP and Kerberos support, on-disk encryption and auditing.

# Document Database
A record in MongoDB is a document, which is a data structure composed of field and value pairs. MongoDB documents are similar to JSON objects. The values of fields may include other documents, aary and arrays of documents.

```json
    {
        name: "sue",
        age: 26,
        status: "A",
        groups: ["news", "sports"]

    }
```

The advantage of using documents are:
* Documents (i.e objects) correspond to native data types in many programming languages.
* Embedded documents and arrays reduce need for expensive joins.
* Dynamic schema supports fluent polymorphism.

## Collections/ Views/ On-Demand Materialized Views
MongoDB stores documents in collections. Collections are analogous to tables in relational databases.

In addition to collections, MongoDB supports:
* Read-only Views(Starting in MongoDB 3.4)
* On-Demand Materialized Views (Starting in MongoDB 4.2)

# Key Features
## High Performance
MongoDB provides high performance data persistence. In particular,
* Support for embedded data models reduces I/O activity on database system.
* Indexed support faster queries and can include keys from embedded documents and arrays.

## Rich Query Language
MongoDB supports a rich a query language to support read and write operations (CRUD) as well as:
* Data Aggregation
* Text search and Geospatial Queries

## High Availability
MongoDB's replication facility, called replica set provides:
* automatic failover
* data redundancy

A replica set is a group of MongoDB servers that maintain the same data set , providing redundancy and increasing data availability.

## Horizontal Scalability
MongoDB provides horizontal scalability as part of its core functionality:
* Sharing distributes data across a cluster of machines.
* Stating in 3.4, MongoDB supports creating zones of data based on the shared key. In balance cluster, MongoDB directs reads and writes covered by a zone only to thosed shared insided the zone.

## Support for MultiStorage Engines
MongoDB supports multiple storage engines
* WiredTiger Storage Engine (including support for Encryption at Rest)
* In-Memory Storage Engine

# Databases and Collections
MongoDB stores BSON documents i.e. data records, in collections, the collections in databases.

## Databases 
In MongoDB, databases hold collections of documents.

To select a database to use, in the mongo shell, issue the `use <db>` statement.

If a database does not exist, MongoDB creates the database when you first store data for that database. As such you can switch to a non-existent database and perform the following operation in the mongo shell:
> use myNewDB
>
> db.myNewCollection.insertOne({x: 1})

The `insertOne()` operation creates both the database **myNewDB** and the collection **myNewCollection** if they do not already exist.

## Collections
MongoDB store documents in collections, Collections are analogous to tables in relational databases.

> db.myNewCollection2.insertOne({x:1})
>
> db.myNewCollection3.createIndex({y: 1})

Both `insertOne()` and `createIndex()` operations create their respective collection if they do not already exist.

### Explicit Creation
MongoDB provides the `db.createCollection()` method to explicitly create a collection with various options, such as setting the maximum size or the documentation validation rules. If you are not specifying these options, you do not need to explicitly create the collection since MongoDB creates new collection when you first store data for the collections.

### Document Validation
By default, a collection does not require its documents to have the same schema; i.e. the documents in a single collection do not need to have the same set of fields and the data type for a field can differ across documents within a collections.

Starting in MongoDB 3.2, however, you can enforce document validation rules for a collection during update and insert operations.

### Modifying Document Structure
To change the structure of the document in a collection, such as add new field, removing existing fields, or change the field values to a new type, update the document to the new structure.

### Unique Identifiers
Collections are assigned an immutable UUID. The collection UUID remains the same across all members of a replic set and shared in a shared cluster.

To retrieve the UUID for a collection, run either the `listCollections` command or the `db.getCollectionInfos()` method.

## Views
A MongoDB view is queryable object whose contents are defined by an aggregation pipeline on the other collections or views. MongoDB does not persist the view contents to disk. A view's content is computed on-demand when a client queries the view. MongoDB can require clients to have permission to query the view MongoDB does not support write operation against views.


