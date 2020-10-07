# Change Dection

The problem with this is that it is tremendously expensive. Changing anything in the application becomes an operation that triggers hundered or thousand of functions looking for changes. This is a fundamental part of what Angular is, and it puts a hard limit on the size of the UI you can build in Angular while remaining performant.

Although with a good understanding of the way angular digest was implemented it was possible to design your application to be quite performant. For example, selectively using `$scope.$digest()` instead of `$scope.$apply()` everywhere and embracing immutable objects. But the fact that knowing under the hood implementation is required to be able to design a performant application is probably a show-stopper for many.

So it's no wonder most tutorials on Angular claim there is no more $digest cycle in the framework. This view largely depends on what exactly is meant by the digest, but I think that given its purpose it's a misleading claim. It's still there. Yes, we don't have explicit scopes and watcher and don't call `$scope.$digest()` anymore, but the mechanism to detect changes that traverses the tree of components, call implicit watchers and updates the DOM has been a given second life in Angular Completely rewritten. Much enchanced.

## The need for digest
Before we begin, let's remember why the digest appeared in the angular.js in the first place. Every framework solves the problem of synchronization between a data model (javascript objects) and UI (Browser DOM). The biggest challenge in the implementation is to know when a data model changed. `The process of checking for change is called change dection`. And it's implementation is the biggest differentiator between most popular framework nowdays. I'm planning to write an in-depth article on change detection mechansims comparison between existing frameworks.

There are two principle ways to detect changes - ask user to notify a framework or detect changes automatically by comparison. Suppose we have the following object: 
```typescript
let person = {name : 'Angular'};
```
and we updated the `name` property. How does a framework know it's been updated? One way is to ask a user to notify a framework:
```typescript
constructor() {
    let person = {name: 'Angular'};
    this.state = person;
}
...
// explicity notifying React about the changes
// and specifying what is about to change.
this.setState({name: 'Changed'});
```
or force home to use a wrapper on properties so the framework can add setters:
```typescript
let app = new Vue({
    data: {
        name: 'Hello Vue!'
    }
});
//the setter is triggered so Vue knows what changed
app.name  = 'Changed';
```
The other way is to save the previous value for the name property and compare it to the current:
```typescript
if(previousValue !== person.name) //change detected
//update DOM
```
But when should the comparison be done ? We should run the check every time the code runs. And since we know that code runs as a result of an asynchronous event - so called Virtual Machine(VM) turn/ tick, we can run this check in the end of the turn. And this what Angular.js use digest for. So we can defines digest as

> a change detection mechanism that walks the tree of components, checks each component for changes and updates DOM when a component property is changed.

If we use this defination of digest, I assert that the primary mechansim hasn't changed in the newer Angular. What changed is the implementation of digest.

## Angular.js


