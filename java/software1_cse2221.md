# Notes - Software I (CSE 2221)
#cs #cse2221

### What is Java
* based on C/C++
* Originally designed for interactive television

### Java Compiler
* Checks source code in a .java file.
* Iff there are no compile-time errors, it generates bytecode for that program and saves it in a .class file

### Java Virtual Machine
* A virtual computer - just like any other program that runs on real physical hardware and OS
* The “launcher” of the JVM for your computer and OS loads the JVM and your program’s .class file(s)
* The JVM executes your program by interpreting the byte code that is loaded
* A launcher called java and the JVM are part of the Java Runtime Environment (JRE) for your computer and OS
* The same bytecode can be run on any Machine that has it’s own JVM
* Provides 
	* portability - JVM is ubiquitous
	* Universality - Program source code need not be in Java
	* Performance - The extra layer is actually fairly performant

### Input / Output
```
import components.simplewriter.SimpleWriter;
import components.simplewriter.SimpleWriter1L;
import components.simplereader.SimpleReader;
import components.simplereader.SimpleReader1L;

public final class HelloWorld {
	private HelloWorld() {
	}
	public static void main(String[] args) {
		SimpleWriter out = new SimpleWriter1L;

		out.println("Hello World!");
		out.close();
		
		SimpleReader keyboardIn = new SimpleReader1L();
		SimpleReader fileIn = new SimpleReader1L("foo.txt");

		String line = keyboardIn.nextLine();
		line = fileIn.nextLine();
		keyboardIn.close();
		fileIn.close();
	}
}
```

### Variables
Program variable - value may change throughout a program
Mathematical variable - arbitrary but fixed value
Naming convention for variables is camelCase
Declare -> Initialize

### Constants
`final` keyword indicates initialization as a constant
`final int MY_LUCKY_NUMBER = 7;`
Naming convention: Screaming snake


### Expressions
An expression is a “syntactically wellformed and meaningful fragment

### Statements
A statement is a “smallest complete unit of execution”
A simple statement is terminated with a semi-colon `;`

### Stacks
A 32-bit operating system has a st

### Heap
storage space for large object that can’t fit within the 32-bit instruction size of the stack

### Assignment Statement
Assignment statement form: variable = expression
This **copies** the value of the expressions on the right hand side of the **assignment operator** to the variable on the left side

### Tracing Tables (While Loops)
![](software1_cse2221/Screen%20Shot%202018-04-27%20at%2011.05.09%20AM.png)


### Blocks / Compound Statements
Any sequence of zero or more statements enclosed in _{…}_ is a **block**
**A block is a compound statement**

### Monte Carlo Estimation
Class of computational methods that use random sampling to estimate results

### Static Methods
* A static method (class method) is a block of code with a name, using which it can be called (invoked) to perform it’s computation
* The method **“takes over”** execution when it is called, until it **returns** to the calling program at the point it was called
* A “private” method can only be called from within the class where it is defined. public methods can be called from other classes as well

### Arguments & Parameters
* Arguments are what you pass into a function call
* Formal Parameters are what appears in the functions signature

### Arrays
initializing to default values with size of four
`int[] a = new int[4];`
initializing to specified values with size of four
`int[] a = { 6, 18, 9, –10 };`

### For-each loop
```
for (String word : dictionary) {
	// do something with word
}
```

### Design-by-contract (programming-to-the-interface)
the standard policy governing “separation of concerns” across modern software engineering
* A **system** is any part of anything that you want to think about as an 
indivisible unit
* An **interface** is a description of the boundary between a system and everything else, also describes how to think about that system as a unit
* A **subsystem** (component) is a system that is used inside of another system
* A **client** is a person (or a role played by some agent) viewing a system “from the outside”
* An **implementer** is a person (or a role played by some agent) viewing the system “from the inside”
* **Information hiding** is a technique for describing system behavior in which you intentionally leave out “internal implementation details” of the system
* **Abstraction** is a technique in which you create a valid cover story to counteract the effects of hiding some internal implementation details

### Structure of a Method Contract
Each method has:
* A **precondition** (requires clause) that characterizes the responsibility of the program that calls (uses) that method (client code)
* A **postcondition** (ensures clause) that characterizes the responsibility of the program that implements that method (implementation code in the method body)
* It is the clients responsibility to ensure that the precondition is true when a methods is called
* The client may assume the postcondition is true when the method returns, this is the responsibility of the implementer

### Javadoc
* the standard documentation technique for Java is called Javadoc
* Javadoc comments enclosed in `/**   ...   */`
* The Javadoc tool will generate nicely formatted web-based documentation from them
* The resulting docs are known as the **API**

```
/**
* ...
* @param x number to take the square root of
* @param epsilon allowed relative error
* @return the approximate square root of x
* @requires
* x > 0 and epsilon > 0
* @ensures <pre>
* sqrt >= 0 and
* [sqrt is within relative error epsilon
* of the actual square root of x]
* </pre>
*/
private static double sqrt(double x, double epsilon)
```

`@param`:  needed for each formal parameter; describes the parameters role in the method
`@return`: is needed if the method returns a value, describe the returned value
`requires`: introduces the precondition for the method
`ensures`: introduces the postcondition for the method
`<pre> ... <pre/>`: these wrap text where spacing / line breaks are to be preserved

Non-mathmatical statements are wrapped in `[ ... ]`

### Assert Statements
```
private static double sqrt(double x, double epsilon) {
	assert x > 0.0 :
	"Violation of: x > 0";
	assert epsilon > 0.0 :
	"Violation of: epsilon > 0";
	// rest of body: compute the square root
}
```
* It is best practice to check assumptions with `assert` when it is easy to do so
* Assert checking can be turned on using the `-ea` argument to the JVM
* Use assert checking during development, generally not doing deployment

### Trees
* the **size** of a tree is the total number of nodes, including the root
* the **height** of a tree is the length of the longest path from it’s root (counting nodes, not edges)

### Subtrees
subtree **i** is the subtree whose root node is the i-th child node of the parent tree’s root node.

### XML Documents (extensible markup language)
Used to represent hierarchically organized data for “ease of use” both by humans and computers

```
<?xml version="1.0" encoding="UTF-8"?>
<book printISBN="978-1-118-06331-6"
webISBN="1-118063-31-7"
pubDate="Dec 20 2011">
<author>Cay Horstmann</author>
<title>
Java for Everyone: Late Objects
</title>
...
</book>
```

* An **xml declaration** is the first line of a well-formed XML file
 `<? version="1.0" encoding="UTF-8"?>`
* Includes a top-level element
* A string of zero or more child elements of the top-level element
* structure is recursive, information represented is hierarchical

### XMLTree
* allows you to create, and navigate through, a tree whose structure mirrors that of an XML file
* takes care of all parsing (recognizing tags, matching start-end tags, etc.)
* The content of a tag is in a child node to the tag’s node. the content node has no child

### XMLTree methods
All methods for XMLTree are instance methods `t.methodName(arguments)`
* `t` is the **receiver**


### References
Some things are stored in a **stack**, some things are stored in a **heap**
**primitive types:** built-in types (boolean, char, int, double)
**reference types:** i.e class types (String, XMLTree, SimpleReader, etc.)
**reference value** -> stores an ID that references a memory address
**object value** -> the value that the reference ID (memory address) points to
**anonymous primitive variable**: in the statement `int i = i + 7`, `7` is an anonymous primitive variable.
**anonymous reference variable**: in the statement `String s = s2 + "io"`, `"io"` is an anonymous reference variable
**uninitialized reference variable**: in the statement `String s;`, The declaration of the String variable `s` results in an uninitialized reference variable
**aliases**: multiple reference values pointing to the same object value are known as aliases.
**immutable types:** types for which no method might change the value of the receiver.
**mutable types:** at least one method might change the value of the receiver.
NOTE: the `==` operator compares _reference values_, this means you can pretty much never use it to compare reference types.


### Items for Midterm 2:
- [ ] Parameter Modes
- [ ] Strings are immutable reference types. the string type has no methods that modify the receiver directly, they only return a new string, and thus must be assigned.
- [ ] one t/f or multiple choice question about parameter modes
- [ ] extends, inherits, implements


### Midterm 2 Key Terms:
**modulo:** clock arithmetic (-67 mod 24 = 5)
**remainder:** the `%` operator. (-67 % 24 = -19) in Java
**kernel methods:** primary methods
**secondary methods:** build off of the kernel methods
**4 constructors for Natural Number:**
		* no argument constructor `new NaturalNumber2()`
		* copy constructor `new NaturalNumber2(k)`
		* constructor from int `new NaturalNumber2(1)`
		* constructor from String `new NaturalNumber2("1")`
**receiver:** the instance upon which an instance method is called
**distinguished formal parameter:** this (what we call the receiver inside of an instance method)
**parameter modes:** 
		* **updates:** the value might be changed, and the methods behavior depends on the incoming value (the type in question is mutable)
		* **replaces:** the value might be changed, but the behavior of the method is independent of the old value (type in question is mutable)
		* **clears:** the variable’s value is reset to an initial value (no-value constructor value) (type in question is mutable)
		* **restores:** the default parameter mode. parameter will retain it’s incoming value
**immutable reference type:** has no methods that modify the receiver directly
**primitive type:** built-in types
**mutable reference type:** may have methods that modify the receiver directly
**reference value:** the memory address at which the object is stored
**object value:** the mathematical model value of the object pointed to by a reference
**Stack**: holds primitive variable values and reference variable reference values
**Heap:** holds object values of reference variables
**assignment operator:** copies the value of the expression on the right-hand side into the variable on the left-hand side
**anonymous reference variable:** a literal object value that is never assigned a reference value `"io"`. this reference variable disappears after the statement where it is used finishes executing.
**uninitialized reference variable:** `String s;`
**uninitialized primitive variable:** `int i;`
**garbage collector:** comes along and reclaims/recycles memory where unreferenced temporary objects are stored
**aliases:** multiple reference values pointing to the same object value.
**array**: (reference types) a group of similar variables, all with the same type (neither `==` nor `.equals()` will work for comparing arrays)
**null reference:** a reference that points to no object value
**simple assignment:** can cause aliasing for reference variables
**parameter passing:** another source of aliasing (reference values of the arguments are copied into the method’s formal parameters)
Never pass any variable of a mutable reference type as an argument twice or more to a single method call (includes the distinguished formal parameter)
**Mathematical Strings:**
	* a series of zero or more entries of any other mathematical type (entry type)
	* if `T` is the entry type, we will call the math type `string of T`
	* `string != String`
	* type `String` is modeled by `string of character`
	* denoted `< 1, 2, 3, 1 >` or `< 'G', 'o' >`
	* empty string (no entries) is denoted `< >` or `empty_string`
	* concatenation of two strings (s and t) is denoted `s * t`
	* **substring:** s is a substring of t if s appears in t consecutively. (a substring starting at position `i`  (inclusive) and ending at position `j` (exclusive) is denoted by `s[i,j]`
	* **prefix:** s is a prefix of t if s appears at the beginning of t.
	* 	**suffix:** s is a prefix of t if s appears at the end of t.
	* **reverse:** same entries as s but in the opposite order `rev(s)`
	* **permutations:** are two strings reordering of one another `perms(s1, s2)`
	* **occurrence count:** `count(s,x)` the number of times `x` appears in as an entry in `s`
**recursive method:** a method that calls itself
**implements:** may hold between a class and an interface
**extends:** may hold between either two interfaces or two classes.
**child interface:** child interface_derived Interface_subinterface
**parent interface:** parent interface_base interface_ superinterface
**child class:** child class_derived class_subclass
**parent class:** parent class_base class_ superclass
**uses for class extension:**
		* To add method bodies that are not already in the class being extended
		* To override methods that are already implemented in the class being extended, by providing new method bodies for them
**Method overriding:** extending a class_interface and providing a new method body for an already existing method in the class_interface being extended
**Method overloading:** when two or more methods have the same name, but different signatures
**Declared type:**
		* **interface type:** when a variables type is the name of an interface (best practice)
		* **class type:** when a variable type is the name of a class as it’s type
**Instantiated type:**
		* **object type (dynamic type):** the class type from which the constructor comes

### Final Exam Key Terms
**Queue:** allows you to manipulate strings of entries of any (arbitrary) type in FIFO (first-in-first-out) order (modeled mathematically by a string of T)
**Generic Type (parameterized type):** the actual type of entries is selected only later by the client when the type Queue is used to declare or instantiate a variable. (must be reference type)
**T:** the generic formal type
**wrapper type:** immutable reference type that wraps a primitive type (for use as a generic type)
**auto-boxing/unboxing:** allows wrapper types to be used with primitive type syntax almost as if they were primitive types.
**Comparator:** a Java interface that has one compare method that takes two generic type parameters and returns -1, 0, or 1.
**Mathematical Set Notation:** 
	* e.g { 1, 42, 13 }
	* No order among elements
	* No duplicate elements
	* empty set: {}
	* set difference: { 1, 2, 3 } \ { 1, 2 } = { 3 }
	* proper subset: the two sets in question cannot be equal
**Set:** allows you to manipulate finite sets of elements or any type
**Iterator:** Set extends the interface iterable, therefore you may write a for-each loop to “see” all of the elements
**Event:** the act of a user manipulating a widget
**Subject:** the widget the user has manipulated
**Observers/Listeners:** objects that need to do something in response to the events for a particular subject
	* Each subject expected an observer to register itself with the subject
	* keeps track of it’s own set of interested observers
	* when an event occurs, the subject invokes a callback method for each registered observer, passing an event argument that describes the event.
**Thread:** a single path of sequential code executing one step at a time
	* A GUI with Swing uses at least two threads (Main thread and event dispatch thread)
**Layout Manager:** allows you to arrange widgets without providing specific location coordinates
**MVC:** 
	* The **Controller** holds a reference to both the model and the view
	* The **View** holds a reference to the controller
	* The **View** extends JFrame and implements action listener.
**Constructor for View Class:** the view constructor has four main jobs:
	* Create the JFrame being extended
	* Set up the GUI widgets to be used and lay out those widgets in the main application window
	* Set up the observers by registering the object being constructed `this` with each of the GUI widgets that might have events of interest in this application
	* Start the main application window
**Loop Invariant:** a property that is true every time the code reaches a certain point. 
``` Java
* @maintains
* this * q = #this * #q
* @decreases
* |q|
```


### Final Exam Topics

Tuesday 10:00 am - 11:45 am in lecture classroom

#### Terminology question concepts (for first 2/3)
design by contract
classes + interfaces
method calls + parameter passing
	* primitives, immutable reference type, mutable reference type
	* diagraming + tracing program execution

**Very important (disproportionate act of questions on these)**
observer pattern
MVC pattern

tracing over a loop body with a loop invariant `@maintains`
Coming up with good test cases

programming questions
Generics/collections
iteration_recursion_




``` Java 
private static int countTagLeaves(XMLTree t) {
	int count = 0;
	if (t.isTag() && t.numberOfChildren() > 0) {
		for (int i=0;i<t.numberOfChildren();i++) {
			count += countTagLeaves(t.child(i));
		}
	} else if (t.isTag()) {
		count++;
	}
	return count;
}

```

``` Java 
private static int digitAt(NaturalNumber n, int pos) {
	int res;
	int digit = n.divideBy10();
	if (pos == 0) {
		res = digit;
	} else {
		res = digitAt(n, pos-1);
	}
	n.multiplyBy10(digit);
	return res;
}

```

``` Java 
private static boolean isSubstring(String str, String substr) {
	boolean res = false;
	for (int i=0;i<str.length - substr.length + 1;i++) {
		int matches = 0;
		for (int j=0;j<substr.length;j++) {
			if (substr.charAt(j) == str.charAt(i+j)) {
				matches++;
			}
		}
		if (matches == substr.length) {
			res = true;
		}
	}
	return res;
}

```




