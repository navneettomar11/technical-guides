# Relations
Relation or Binary Relation R from set A to B is a subset of AxB which and defined as 
aRb ↔ (a,b)€ R ↔ R(a,b)
A Binary Relation R on a single set A is defined as a subset of AxA. For two distinct set A and B with cardinalities m and n, the maximum cardinality of the relation R from  A to B is mn.

A relation is a relationship between sets of values. In math, the relation is between the x-values and y-values of ordered pairs. These set of all x-values is called the domain and the set of all y-values is called the range.

A relation from a set A to be a set B is a subset AxB. Hence, a relation R consists of ordered pairs (a,b), where a € A and b € B. If (a,b) € R we say that is related tom and we also write aRb.

Let A be a set of students and let B be a set of courses. Given a € A and b € B, define "a is related to b" if and only if student `a` is taking course `b`. While it could be possible that "John Smith" is related to "MATH 210" because John is taking MATH 210, it certainly absurd say that "MATH 210" is related to John Smith because it does not make much sense to say that MATH 210 is taking John smith. This again illustrates that a is related to b does not necessarily imply that b is also related to a.

## Domain and Range
If there are two sets A and B and Relation from A to B is R(a,b), the domain is defined as the set {a | (a,b) € R for some b in B} and Range is defined as the set { b | (a,b) € R from some a in A}.


## Type of relation
1. **Empty Relation**: A relation R on a setA is called Empty if the set A is empty set.
2. **Full Relation**: A binary relation R on a set A and B is called full if AxB.
3. **Reflexive Relation**: A relation R on a set A is called reflexive if (a,a) € R holds for every element a € A i.e. if set A = {a,b} then R = {(a,a), (b,b)} is reflexive relation. A reflexive relation is the one in which every element maps to itself. For example, let us consider a set A = {1,2}. Now here the reflexive relation will be R = {(1,1),(2,2),(1,2), (2,1)}
4. **Irreflexive relation**: A relation R on a set A is called irreflexive if no (a,a) € R holds for every element a € A i.e. if set A = {a,b} then R = {(a,b), (b,a)} is irreflexive relation. A relation R on set A is called irreflexive if no a € A is related to a (aRa does not hold). 
5. **Symmetric Relation**: A relation R on a set A is called symmetric if (b,a) € R hold when (a,b)€ R i.e. The relation R = {(4,5), (5,4), (6,5), (5,6)} on set A = {4,5,6} is symmetric.
6. **AntiSymmetric Relation**: A relation R on a set A is called anti-symmetric if (a,b) € R and (b,a) € R then a b is called antisymmetric i.e. The relation R = {(a,b) -> R|a <=b} is anit-symmetric since a<=b and b<=a implies a = b.
7. **Transitive Relation**: A relation R on a set A is called transitive if (a,b) € R and (b,c) € R then (a,c) € R for all a,b,c € A i.e. Relation R = {(1,2),(2,3), (1,3)} on set A = {1,2,3} is transitive.
8. **Equivalence Relation**: A relation is an equivalence relation if it is reflexive, symmetric and transitive i.e relation R = {(1,1),(2,2),(3,3),(1,2),(2,1),(2,3),(3,2),(1,3),(3,1)}  on set A = {1,2,3} is equivalence relation as it is reflexive, symmetric and transitive.
9. **Asymmetric relation**: Asymmetric relation is opposite of symmetric relation. A relation R on a set A is called asymmetric if no (b,a) € R when (a,b) € R.