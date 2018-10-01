# C++ Notes
#code #cs

### Language Structure:
**Objects** - objects have properties and methods. an object is an instance of a class
**Class** - a template / blueprint that defines the behaviors_states_methods that objects of it’s type support
**Instance Variables** - each object has a unique set of instance variables. An objects state is created by the values assigned to these instance variables

### Program Structure
```
// including the iostream header
#include <iostream> 
// tells this program to use the std namespace
using namespace std; 

// main() is where program execution begins.
int main() {
   cout << "Hello World" << endl; // prints Hello World
   return 0;
}
```


### Primitive Built-in types
	* Boolean: `bool`
	* Character: `char`
	* Integer: `int`
	* Floating point: `float`
	* Double floating point: `double`
	* Valueless: `void`
	* Wide character: `wchar_t`
Note: Some types can be modified using the following modifiers:
	* Signed
	* Unsigned
	* Long
	* Short

### Enumerated Types
enumerated types can be created to have a discreet list of predefined values. 
`enum enum-name { list of names } var-list; `
```
enum color { red, green, blue } c;
c = blue;
```

### Variables
Declare >> Define >> Initialize
``` variable declaration
extern int a,b;
```
``` variables definition
int    i, j, k;
char   c, ch;
float  f, salary;
double d;
```
``` variable initialization
i = 10;
j = 20;
```
Note: variable declaration is used to assure the compiler that a variable of a given type and name will be present during linking so that it can proceed without throwing any errors. This is useful if the variable is defined in another file.

Local variables: declared inside a function or a block
Global variables: defined outside of all functions/blocks

Note: When a local variable is defined, it is not initialized by the system, you must initialize it yourself. Global variables are initialized automatically by the system when you define them

### Lvalues and Rvalues
Lvalue: expressions that refers to a memory location
Rvalue: expression that refers to data that exists in a memory location that cannot be assigned to
`int g = 20; // g is an Lvalue and 20 is an Rvalue`

### Constants 
can be defined using either
 `#define LENGTH 10` 
or
 `const int LENGTH = 10`

### Class
Static variables_methods: using the “static” keyword in a public variable or method declaration means that the variable_method is associated at the class level instead of the object level.
* Static variables must be initialized at the top of the cpp file where that the class is implemented in
```
class Test {
	private:
		int id;
		static int count;
	public:
		Test() {
			id = count++;
		}
		int getId() {
			return id;
		}
};
// initialize the static variable for count
int Test::count = 0;
```

 