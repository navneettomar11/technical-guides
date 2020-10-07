# Data Modeling
The key challenge in data modeling is balancing of the application, the performance characteristics of the database engine, and the data retrieval patterns. When designing data models, always consider the application usage of the data(i.e. queries, updates and processing of the data) as well as the inherent structure of the data itself.

## Flexible Schema
Unlike SQL database, where you must determine and declare a table's schema before inserting data. MongoDB's collections, by default, does not require its document to have the same schema. That is:
* The document in a single collection do not need to have the same set of fields and the data type for a field can differ across documents within a collection.
* To change the structure of the documents in a collections, such as add new field, remove existing fields, or change the field values to a new type, update the documents to the new structure.

The flexibility facilitates the mapping of documents to an entity or an object. Each document can match the data fields of the represented entity, even if the document has substantial variation from other documents in the collection.

## Document Structure
The key decision in designing data models for MongoDB applications revolves around the structure of documents and how the application represent relationship between data. MongoDB allows related data to be embedded within single document.

