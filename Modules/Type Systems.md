# Structural equivalence
Things are structurally equivalent if they share the same built in type or they are both pointers to things that are structurally equivalent.
Type a: int
Type b: int
type c : c*
type d: b*
A and b are the same, and c and d are the same because a and b are the same.
Given a structure they are the same if:
- They have the same number of elements
- each element in order is structurally equivalent.
Arrays by extension are equivalent given the rules above and that each element has the same number of dimensions.
Functions work the same but given the parameters and return type.
# Name equivalence
Two types are equal if they have the same name are two anonymously declared types could never be the same unless they were both declared using the same anonymous declaration.
ie
a,b : array of int
c: array of int
a and b are equivalent but a/b and c are not equivalent.
# Internal Name Equivalence
Never actually mentioned in lecture. thanks
# Polymorphism
Passing types as parameters is expliset polymorphism. This is how generic functions are created typically. Math operations must use implicit polymorphism as it makes no since to include this information when writing a formula. You can also have an explisit polymorphism by having multiple footprints for the same function. ie a function that is named the same as another but has different parameters and/or different return type.
## Hindley- Milner Type Inference
Given undeclared information how to determine the type of the values that make up the statement.
x = y + z
z = 1+ a[b]
what can we say about the types? b must be an intiger as only ints can be used as indexes. a must be an array of ints because we add it to one. z must be a int as it is equivilent to a product of two ints. y is an int because it is added to an int. and x is an int because it is a product of two ints.
[[Homework 2/Problem 2|Problem 2]]
function definitions are expressed (parameters)=>return
(int,int)=>bool