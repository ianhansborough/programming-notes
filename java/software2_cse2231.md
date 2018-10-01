# Notes - Software II (CSE 2231)
#cs #cse2221

### Review
* interfaces have contracts and method headers
* interfaces do not have method bodies (classes that implement them have those)

### Abstract Classes
* contains the bodies of some but not all of the methods of the interfaces it claims to implement. 
* Cannot be instantiated (b/c they are incomplete
* Everything extends the object class
* 

### Object Class
* equals method
* hashCode method 
* toString method

### Protected method
`protected abstract void someMethod() {...}`
	* means this method may be called or overridden in a subclass but may not even be called from any other class declared outside of this package.

### Abstract method
`abstract void someMethod() {...}`
	* this method must be overridden in some subclass.


### Midterm 1 Prep:
**Abstract class:** a “partial” or “incomplete” class that contains bodies of some (but typically not all) of the methods of the interfaces it claims to implement
**Layered method bodies:** method bodies from a parent class or interface that have been overridden.
**Varargs**: `public void (...String args) {...}`
**Using an interface:** a class is said to “use” an interface if one of it’s private members implements said interface (i.e NaturalNumber2 “uses” the Stack interface)
**abstract state space:** the set of all possible math model values as seen by the client
**concrete state space:** the set of all possible math model values of the data representation (seen by the implementer).
**commutative diagram:** shows both the abstract and concrete state spaces immediately before and after a method call
**built in arrays:** cannot declare built in array of generic type or a structure containing a generic type `Array<Integer> ai = new Array1L<>(3);`
**hash table:** the array of buckets you are hashing into (technically this is only for chained hashing)
**binary tree (mathematical):** consists of a root node and two subtrees. it’s structure is recursive.
		* size: total number of nodes in t
		* height: the number of nodes on the longest path from the root to an empty subtree of t
		* labels: the set whose elements are exactly the labels of t
		* compose (binary tree): the mathematical model function used to represent the construction of a binary tree.
**binary tree:**
		* the dynamic type of `this` must be the same as the dynamic type of `left` and `right`
		* pre-order: root is visited before left and right
		* in-order: root is visited between left and right
		* post-order: root is visited after left and right
**binary search tree:** may be used to search for items of any type which has defined a total preorder (a binary relation on T that is total, reflexive, and transitive)

### Heaps + Heapsort
a binary tree with two properties
	1. it is a complete binary tree (all levels are full except for maybe the last level. any nodes on the bottom level are moved as far left as possible)
	2. The label in each node is <= the label in each of it’s child nodes

a heap can be used to sort values by arranging a list of values into a heap
then, removing 


