# The Weird Parts of JavaScript

## JavaScript Engine
* The JS engine is a Process Virtual Machine (a software-driven emulation of a specific process) that allows for the interpretation and execution of JavaScript code.  The sole purpose of the JS Engine is to read and compile code written by a developer into optimized code that can be interpreted by the browser.  The goal of any JS Engine is to parse through and generate the most optimized code as fast as possible to minimize run-time delay.
## JavaScript Execution Context
* The environment in which code is executed in regards to the value of `this`, variables, objects, and functions.  
1. Global Execution Context
   * The initial JS execution context when the file first loads to the browser.  There is only one GEC in which all other Functional Execution Contexts live.  
2. Functional Execution Context
   * The execution context initiated when a function is invoked.  Each function has its own execution context and has access to any code within the invoked function's lexical scope.
## JavaScript Execution Context Stack
* The JS Engine parses through the code creating an ordered stack of executable code based on the code's context.  The stack always begins with the Global Execution Context, adding Functional Execution Contexts on top of the GEC.  As the JS engine parses through the code and reads a function call, a new FEC is added to the top of the execution stack.  The JS engine executes from the top of the stack down and once a function task is completed it is popped off the top of the stack and the next function is executed.  
## KEEP IN MIND
* The stack is single threaded, synchronously executable, meaning that a single function can be created and activated at one time.  
* Their are two phases within an Execution Context; Creation and Activation.  
* The Creation Stage begins with the outer most context, the global context, and works towards the inner most nested functions.  
* Like discussed earlier the global context sits at the bottom of the stack and each preceding execution context is added on top of the last.  
* Once each context is created the activation phases pops functions off the top os the stack once their task is complete.  
* The creation phase runs bottom to top, and the activation phase runs top to bottom.
* The execution directions of the creation and activation phases explain the reason why hoisting allows for functions to be called before their declaration.       
## Execution Context Stages
1.  Creation Stage
   * The JS engine parses through the code and when a function is parsed a new execution context is created and added to the stack.  When that function's execution context is created a `variable object` and an `argument object` are both created for the individual function.  Within the `variable object`, nested functions are added followed by declared variables.  The hoisted nested functions point to their own execution context, while the variables are hoisted as `undefined`.  At this point the value of `this` is established within the lexical scope of the function.  
2. Activation Stage
   * The stack runs all nested functions within the execution context and assigns variables their values.

* Example of Creation and Activation Phases
```JavaScript
 function firstEC(i) {
   //this is the first functional execution context created on the stack
   var first = 'first execution context Variable 1';
   //"undefined" upon creation, and then given value upon activation
   console.log(first);
   var second = 'first execution context Variable 2';
   //"undefined" upon creation, and then given value upon activation
   console.log(second);
   var thirdEC = function() {
     //"undefined" upon creation, and then given value upon activation.  Upon activation, this is function statement for the third execution context
     console.log('third execution context through function call, and firstEC argument:' + i);
   };
   secondEC();
   //this function call within the creation stage of "firstEC" points to a function creating a second execution context
   thirdEC();
   //this function call within the creation stage of "firstEC" points to a function creating a third execution context
   function secondEC() {
     //this is function statement for the second execution context  
     console.log('second execution context through function call, and firstEC argument:' + i);
   }
 };
 console.log('global execution context ');
 //this is apart of the global execution context
 firstEC(1);
 //this function call points to a function creating the first execution context
```          
## Definitions
### Lexical Scope
* Inner functions contain the scope of their parent function, meaning that the child function has access to the values held within its parent function, even if if the parent function has returned (this is all based on the position of declared code with an execution context).  
### Object
* A collection of properties with key/value pairs or methods which are declared functions within the object's collection.  An object can also have a collection that is another object.  
### Object Literal
* When the JS Engine is parsing through your code and comes upon `{}` outside of a function or conditional, the JS Engine assumes that a new object is being created.  Object literal syntax allows the programmer to create an object on the fly with `{}` whenever wanted.  
### Namespace
* A container for variables and functions to be held so that separate variables and functions, possibly with the same name, do not cause conflict when called.  JS doesn't have a specialty namespace syntax, but objects can be used to accomplish this task.  
### JSON (JavaScript Object Notation)  
* A string of data that is almost identical to object literal syntax except all property names have to be wrapped in quotes.  JSON is a subset of object literal syntax, meaning that all object literal cannot be used as JSON, but all JSON can be used as object literal.  
### First Class Functions
* Functions act differently in the JS Engine compared to other languages, because everything you can do with other types (string, number, boolean, etc.) you can do with functions.  Functions can be assigned to variables, passed around, and created on the fly.  Functions are objects in JS.  It is a special type of object with 2 special properties that regular objects do not have.  The name property, that is optional, and the code property that holds the code to be preformed when the function is invoked.  
### Function Expression
* A variable that holds an anonymous function.  A function expression cannot be invoked before it is declared because the variable is hoisted as `undefined`.
```JavaScript
  var greeting = function() {
    console.log('hi!');
  };
  greeting();
```    
### Function Statement
* A function with a declared name.  A function statement is hoisted with invokable code, allowing the function to be called before it is declared within the code resulting in a value other than `undefined`.
```JavaScript
  function greet() {
    console.log('hello!');
  };
  greet();
```
### By-Value
* When a variable is declared and defined, that variable is saved to memory pointing to the defined value.  If a separate variable is declared and set equal to the initial variable, then a copy of the initial variable is created and the second variable points at the copied value.  If the initial variables value is redefined, the second variable's value does not change because it is still pointing at the copy of the original value.  
### By-Reference
* A variable that is set equal to a pre-existing object does not cause a copy of that object to be made and saved to memory, instead the variable referencing the object just continually points at the same key/value pairs.  If the object is mutated in any way the newly created variable references those changes because it is still pointed at the original object.  
### `this`
* Basic `this`

   `this` is used to hold the value of a single object.  If `this` is used within a function, then `this` is being used to access the methods or properties of the object that called the function. `this` can only reference the methods and properties within the single object where invoked.   

```JavaScript
  var friends = {
    best: 'George',
    second: 'Larry',
    third: 'Sara',
    myFriends: function() {
      console.log("These are my friends " + this.best + " " + this.second + " " + this.third +".");
    }
  };
  friends.myFriends();
```       
* Callbacks and `this`

   When an object's method is passed to a separate object as a callback, for instance on a click event, if `this` is used to reference a property within the object method being passed, no longer does `this` refer to the correct property.  If `this` is used within a method for a callback function, then `this` now refers the the properties of the invoking function, for instance the button click.

```JavaScript
var allFriends = {
  friends: [
    {name: 'George'},
    {name: 'Larry'},
    {name: 'Sara'}
  ],
  myFriends: function() {
    var rando = (Math.floor(Math.random()*3) + 1);
    console.log("One of my friends is " + this.friends[rando].name + ".");
  }
};
$(button).click(allFriends.myFriends());
//"this" refers to the button, not allFriends, because the button was the invoked function  
```
### `that`
* If an inner method is trying to access an outer function's `this`, then `this` must be saved to a new variable before the inner method is invoked.  This means that a new variable, `that`, points to the properties and methods of the outer object so that the values can be accessed within the closure, otherwise, using `this` within a closure returns `undefined`.  
```JavaScript
  var games = {
    game: 'soccer',
    leagues: [
      {name: 'EPL'},
      {name: 'MLS'},
      {name: 'La Liga'}
    ],
    soccerLeagues: function() {
      var that = this;
      this.leagues.forEach(function(league){
        console.log(league.name + " is a professional " + that.game + " league.");
      })
    }
  };
  games.soccerLeagues();
  //"this" works only one level into an invoking object, once a closure is invoked,
  //"that" must be used to refer to the original invoking object.
```   
### Argument
* A value passed into a function through the function's declared parameter(s).  When the function is declared and hoisted in the create phase of the execution context, the JS Engine sets `undefined` values as place holders for the declared parameters of the function.  Parameters are set to `undefined` initially in order to mitigate an error if the function is invoked without all parameters set arguments.  If the function is possibly passed more arguments than parameter placeholders declared, `...` followed by a name all declared after the set function parameters allows for an array to be created.  The name given after the `...` is the name of the array.  That array holds any additional arguments passed to the function that did not have declared parameter placeholders.  The arguments array can be search just like any array with bracket indexing.  
```JavaScript
  var names = function (first, last, ...other) {
    console.log('Hello, my name is ' + first + " "+ last);
    console.log(other);
  };
  names('James', 'Man', 'Jr.', 'Alan');
```      
### Array Literal
* A collection of numbers, strings, booleans, objects, functions, and other arrays saved to a variable.  Arrays can invoke functions saved within itself, and pass values also saved internally to that same function as a parameter.  
```JavaScript
  var array = [
    {
      name: "Tony",
      age: 29
    },
    function (name) {
      var greeting = "Hello ";
      console.log(greeting + name);
    }
  ];
  array[1](array[0].name);
```
### Immediately Invoked Function Expressions (IIFE)
* A function expression that is created and immediately passed a parameter value, and executed.  That executed function expression immediately saves a value to a variable.  
```JavaScript
  var greeting = function (name) {
    return 'Hello ' + name + "!";
  }('Tony');
  console.log(greeting);
  //when logged, "greeting" has already been invoked upon //creation, so the function doesn't need to be called with //"greeting()".  The variable "greeting" is no longer equal to a //anonymous function, but now saves a value equal to "Hello //Tony!".  
```      
### Immediately Invoked Function Statement (IIFS)
* An anonymous function can also be used to immediately invoke a value by wrapping the entire function statement in `()`.  This approach is heavily used in modern frameworks to execute code on the fly as the function is created.  This approach places all framework specific code within the same execution context, limiting the possibility for conflicting code because framework specific expression no longer sit on the global execution context.  
```JavaScript
(function (j) {
  var greeting = 'Hello ';
  console.log(greeting + j + "!");
}('Tony'));
```    
### Closure
* When a function is invoked, that function is added to the execution stack and must be completed before the JS Engine can move onto the next task.  If a function is nested within another function, the outer function is executed and any realized value is then passed to the inner function "enclosed" by the outer function.  A closure represents the act of a nested function accessing parent function parameters that hold pertinent value to execute inner function tasks.  
```JavaScript
  function greeting(whatToSay) {
    return function(name) {
      console.log(whatToSay + name);
    }
  }
  var sayHi = greeting('Hi ');
  sayHi('Tony');
  //"sayHi" effectively is "greeting('Hi ')('Tony')", which means
  //that "Hi " is being passed to "whatToSay", causing "greeting"
  //to be executed, completed, and the result to be saved for the
  //anonymous inner function.  For the inner function "whatToSay"
  //is equal to "Hi " and continues to parse through the code
  //passing "Tony" as the value for the "name" parameter, resulting
  //in "Hi Tony"
```
