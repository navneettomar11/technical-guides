# Mapping Operator in RxJS
Some of the most commonly used RxJS operators that we find on a daily basis are the RxJS high-order mapping operators: switchMap, mergeMap, concatMap and exhaustMap.
For example, most of the network calls in our program are going to be done using one of these operators, so gettings familiar with them is essential in order to write any reactive program.
Knowing which operator to use in a given situation (and why) can be a bit confusing, and we often wonder how do these operators really work and why they are named like that.
These operators might seem unrelated, but we really want to learn them all in one go, as choosing the wrong operator might accidentally lead to subtle issues in our programs.

## Why are the mapping operators a bit confusing ?
There is a reason for that: in order to understand these operators, we need to first understand the Observable combination strategy that each one uses interanlly.
Instead of trying undertand switchMap on its own, we need to first understand what is observable switching; instead of diving straight into concatMap, we need to first learn Observable concatentation, etc.

## The RxJS Map Operator
