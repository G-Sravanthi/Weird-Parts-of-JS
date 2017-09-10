# The Weird Parts of JavaScript
## Definition
### Object:
A collection of properties with key/value pairs or methods which are declared functions within the object's collection.  An object can also have a collection that is another obkect.  
### Object Literal:
When the Js Engine is parsing through your code and comes upon `{}` outside of a function or conditional, the JS Engine assumes that a new object is being created.  Object literal syntax allows the programmer to create an object in the fly with `{}` whenever wanted.  
### Namespace:
A container for variables and functions to be held so that separate variables and functions, possibly with the same name, do not cause conflict when called.  JS doesn't have a specialty namespace syntax, but objects can be used to accomplish this task.  
### JSON (JavaScript Object Notation):  
A string of data that is almost identical to object literal syntax except all property names have to be wrapped in quotes.  JSON is a subset of object literal syntax, meaning that all object literal cannot be used as JSON, but all JSON can be used as object literal.  
### First Class Functions:
Functions act differently in the JS Engine compared to other languages, because everything you can do with other types (string, number, boolean, etc.) you can do with functions.  Functions can be assigned to variables, passed around, and created on the fly.  Functions are objects in JS.  It is a special type of object with 2 special properties that regular objects do not have.  The name property, that is optional, and the code property that holds the code to be preformed when the function is invoked.  
### Function Expression:
A unit of code that results in the creation of a value that doesn't necessarily need to save in a variable.  
### Function Statement:
A unit of code that does work on expressions within the statement resulting in a value that is due to the specific work being done to the expressions(s).  A function statement is code that is put into memory, but does not result in a value.  The statement gets hosted and is available in memory.        
