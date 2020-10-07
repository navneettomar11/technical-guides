# Validating Data with JSON-Schema

When you're dealing with complex and structure data, you need to determine whether the data is valid or not. JSON-Schema is the standard of JSON documents that describe the structure and the requirement of your JSON data. 
Let's say you have a database of users where each record looks similar to this example:
```json
{
  "id": 64209690,
  "name": "Jane Smith",
  "email": "jane.smith@gmail.com",
  "phone": "07777 888 999",
  "address": {
    "street": "Flat 1, 188 High Street Kensington",
    "postcode": "W8 5AA",
    "city": "London",
    "country": "United Kingdom"
  },
  "personal": {
    "DOB": "1982-08-16",
    "age": 33,
    "gender": "female"
  },
  "connections": [
    {
      "id": "35434004285760",
      "name": "John Doe",
      "connType": "friend",
      "since": "2014-03-25"
    },
    {
      "id": 13418315,
      "name": "James Smith",
      "connType": "relative",
      "relation": "husband",
      "since": "2012-07-03"
    }
  ],
  "feeds": {
    "news": true,
    "sport": true,
    "fashion": false
  },
  "createdAt": "2015-09-22T10:30:06.000Z"
}
```

The question we are going to deal with is how to determine whether the record like the one above is valid or not.

Examples are very useful but not sufficient when describing your data requirments. JSON-Schema come to the rescue. This is one of the possible schemas describing a user record:
```json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "id": "http://mynet.com/schemas/user.json#",
  "title": "User",
  "description": "User profile with connections",
  "type": "object",
  "properties": {
    "id": {
      "description": "positive integer or string of digits",
      "type": ["string", "integer"],
      "pattern": "^[1-9][0-9]*$",
      "minimum": 1
    },
    "name": { "type": "string", "maxLength": 128 },
    "email": { "type": "string", "format": "email" },
    "phone": { "type": "string", "pattern": "^[0-9()\\-\\.\\s]+$" }, 
    "address": {
      "type": "object",
      "additionalProperties": { "type": "string" },
      "maxProperties": 6,
      "required": ["street", "postcode", "city", "country"]
    },
    "personal": {
      "type": "object",
      "properties": {
        "DOB": { "type": "string", "format": "date" },
        "age": { "type": "integer", "minimum": 13 },
        "gender": { "enum": ["female", "male"] }
      }
      "required": ["DOB", "age"],
      "additionalProperties": false
    },
    "connections": {
      "type": "array",
      "maxItems": 150,
      "items": {
        "title": "Connection",
        "description": "User connection schema",
        "type": "object",
        "properties": {
          "id": {
            "type": ["string", "integer"],
            "pattern": "^[1-9][0-9]*$",
            "minimum": 1
          },
          "name": { "type": "string", "maxLength": 128 },
          "since": { "type": "string", "format": "date" },
          "connType": { "type": "string" },
          "relation": {},
          "close": {}
        },
        "oneOf": [
          {
            "properties": {
              "connType": { "enum": ["relative"] },
              "relation": { "type": "string" }
            },
            "dependencies": {
              "relation": ["close"]
            }
          },
          {
            "properties": {
              "connType": { "enum": ["friend", "colleague", "other"] },
              "relation": { "not": {} },
              "close": { "not": {} }
            }
          }
        ],
        "required": ["id", "name", "since", "connType"],
        "additionalProperties": false
      }
    },
    "feeds": {
      "title": "feeds",
      "description": "Feeds user subscribes to",
      "type": "object",
      "patternProperties": {
        "^[A-Za-z]+$": { "type": "boolean" }
      },
      "additionalProperties": false
    },
    "createdAt": { "type": "string", "format": "date-time" }
  }
}
```
Have a look at the schema above and the user record it describes (that is valid according to this schema). There is a lot of explaining to do here.

Javascript code to validate the user record against the schema could be:

```javascript
var Ajv = require('ajv');
var ajv = Ajv({allErrors: true});
var valid = ajv.validate(userSchema, userData);
if (valid) {
  console.log('User data is valid');
} else {
  console.log('User data is INVALID!');
  console.log(ajv.errors);
}
```
or better performance : `javascript var validate = ajv.compile(userSchema); var valid = validate(userData); if (!valid) console.log(validate.errors);`

[Ajv](https://github.com/tutsplus/ajv), the validator used in the example, id the fastest JSON-Schema validator for Javascript.

## Why validate Data as a Separate step ?
- to fail fast
- to avoid data corruption
- to simplify processing code
- to use validation code in tests

## Why JSON (and not XML)
- as wide adoption as XML
- easier to process and more concise than XML
- dominates web development because of Javascript

## Why Use Schemas?
- declarative
- easier to maintain
- can be understood by non-coders
- no need to write code, third party open-source libraries can be used.

## Why JSON-Schema?
- the widest adoption among all standard for JSON validation
- very mature (current version is 4, there are proposals for version 5)
- covers a big parts of validation scenarios
- use easy-to-parse JSON documents for schemas
- platform independent
- easily extensible
- 30+ validators for different languages, including 10+ for Javascript, so no need to code it yourself.

## Let's Dive Into the Schemas!
JSON-Schema is always an object. It properties are called `keywords`. Some of them describe the rules for the data (e.g., "type" and "properties"), and some describe the schema itself ("$schema", "id", "title", "description").

### Data properties
Because most JSON data consists of objects with multiple properties, the keyword "properties" is probably the most commonly used keyword. It only applies to objects.
You might have notices in the example above that each property inside the "properties" keyword describes the corresponding property in your data.
The value of each property it itself a JSON-scheme- JSON-schema is a recursive standard. Each property in the data should be valid according to the corresponding schema in the "properties" keyword.

The important thing here is that the "properties" keyword doesn't make any property required; it only defines schemas for the properties that are present in the data.

For example, if our schema is:
```json
{
  "properties": {
    "foo": { "type": "string" }
  }
}
```
then object with or without property "foo" can be valid according to this schema:
```json
{foo: "bar"}, {foo: "bar", baz: 1}, {baz: 1}, {} // all valid
```
and only objects that have property foo that is not a string are invalid:
```json
{ foo: 1 } // invalid
```

### Data Type
It is probably the most important keyword. Its value (a string or array of strings) defines what type(or types) the data must be to be valid.

As you can see in the example above, the user data must be an object.

Most keywords apply to certain data types - for example, the keyword "properties" only applies to objects, and the keyword "pattern" only applies to strings.

What does "apply" mean? Let's say we have a really simple schema:
```json
{ "pattern": "^[0-9]+$" }
```
You may expect that to be valid according to such schema, the data must be a string matching the pattern
```json
"12345"
```
But the JSON-schema standard specifies that if a keyword doesn't apply to the data type, then the data is valid according to this keyword.That means that any data thist is not of type "string" is valid according to the schema above - numbers, array, objects, boolean, and even null. If you want only strings matching the pattern to be valid, your schema should be:
```json
{
  "type": "string",
  "pattern": "^[0-9]+$"
}
```
Because of this, you can make flexible schemas that will validate multiple data types.

Look at the property "id" in the user example. It should be valid according to this schema:
```json
{
  "type": ["string", "integer"],
  "pattern": "^[1-9][0-9]*$",
  "minimum": 1
}
```
This schema requires that the data to be valid should be either a "string" or an "integer". There is also the keyword "pattern" that applies only to strings: it requires that the string should consist of digits only and not start from 0. There is the keyword "minimum" that applies only to numbers; it requires that the number should be not less than 1.

Another, more verbose, way to express the same requirment is:
```json
{
  "anyOf": [
    { "type": "string", "pattern": "^[1-9][0-9]*$" },
    { "type": "integer", "minimum": 1 },
  ]
}
```
But because of the way JSON-schema is defined, this scheme is equivalent to the first one, which is shorter and faster to validate in most validators.

Data types you can use in schemas are "object", "array", "number", "integer", "string", "boolean", and "null". Note that "number" includes "integer" - all integers are number too.

