# Queries and Mutations

## Fields
At its simplest, GraphQL is about asking for specific fields on objects. Let's start by looking at a very simple query and the result we get when we run it:

Input: 
```typescript 
{
    hero {
        name
    }
}
```
Output:
```typescript
{
    "data" : {
        "hero" : {
            "name" : "R2-D2"
        }
    }
}
```
You can see immediately that the query has exactly the same shape as the result. This is essential to GraphQL, because you always get back what you expect, and the server knows exactly what fields the client is asking for.

The field `name` returns a `String` type, in this case the name of the main hero of Star Wars, `"R2-D2"`.

> Oh, one more thing - the qery is above *interactive*. That means you can change it as you like and see the new result. Try adding an **appearsIn** field to the hero object in the query, and see the new result.

In the previous ex

