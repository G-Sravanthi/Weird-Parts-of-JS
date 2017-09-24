# The Weird Parts of JavaScript

## JavaScript Engine
* The JS engine is a Process Virtual Machine (a software-driven emulation of a specific process) that allows for the interpretation and execution of JavaScript code.  It acts as a global wrapper that places all of an application's JS code within a global execution context. The sole purpose of the JS Engine is to read and compile code written by a developer into optimized code that can be interpreted by the browser.  The goal of any JS Engine is to parse through and generate the most optimized code as fast as possible to minimize run-time delay.
## JavaScript Execution Context
* The environment in which code is executed in regards to the value of `this`, variables, objects, and functions.  
1. Global Execution Context
   * The initial JS execution context when the file first loads to the browser.  There is only one GEC in which all other Functional Execution Contexts live.  
2. Functional Execution Context
   * The execution context initiated when a function is invoked.  Each function has its own execution context and has access to any code within the invoked function's lexical scope.
## JavaScript Execution Context Stack
* The JS Engine parses through the code creating an ordered stack, or queue, that keeps track of executable code based on the code's context (where in the application a function was invoked).  The stack always begins with the Global Execution Context, adding Functional Execution Contexts on top of the GEC.  As the JS engine parses through the code and reads a function call, a new FEC is added to the top of the execution stack.  A nested function creates a new FEC on top of their parent's FEC within the stack. The JS engine executes from the top of the stack down and once a function task is completed it is popped off the top of the stack and the next function is executed.  
## KEEP IN MIND
* The stack is single threaded, synchronously executable, meaning that a single function can be created and activated at one time.  
* Their are two phases within an Execution Context; Creation and Activation.  
* The Creation Stage begins with the outer most context, the global context, and works towards the inner most nested functions.  
* Like discussed earlier the global context sits at the bottom of the stack and each preceding execution context is added on top of the last.  
* Once each context is created the activation phases pops functions off the top os the stack once their task is complete.  
* The creation phase runs bottom to top, and the activation phase runs top to bottom.
* The execution directions of the creation and activation phases explain the reason why hoisting allows for functions to be called before their declaration.       
## Execution Context Phases
1.  Creation Phase
   * The JS engine parses through the code and when a function is parsed a new execution context is created and added to the stack.  When that function's execution context is created a `variable object` and an `argument object` are both created for the individual function.  Within the `variable object`, nested functions are added followed by declared variables.  The hoisted nested functions point to their own execution context, while the variables are hoisted as `undefined`.  At this point the value of `this` is established within the lexical scope of the function.  The creation phase is used by the JS Engine to establish context within the code, as well as, create memory to store variables and functions necessary for the execution phase.    
2. Activation Phase
   * Code is read line by line executing the code in the order it is read.  The stack runs all nested functions within the execution context and assigns variables their values.

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
##Event Queue
* The stack for managing events triggered by handlers.  Once the execution stack has completed the creation and execution phases , the application runs the event Queue asynchronously, meaning that when an event is called, it is added to the stack awaiting processing power, and once processed, the event is fulfilled (dropdown menu), or waits a response (http request).  If an event is not immediately completed due to outside sources(API), the application continues to process until the necessary response is returned.  These events are used to manipulate the DOM, or manage HTTP requests and responses.    Events run when they are called and there is data to be processed, not based on the sequential order of the written code.               
## Definitions
### Syntax Parser
* Software that reads through code and determines what is to be done.  A syntax parser allows code to be written in script that is more easily written and read by humans, then translated into a lower level computer language to be run.
### Lexical Environment
* The location of each segment of code within the context of the entire application.   
### Lexical Scope
* The context of a piece of code called to run.  Inner functions contain the scope of their parent function, meaning that the child function has access to the values held within its parent function, even if the parent function was returned (this is all based on the position of declared code with an execution context).
### Execution Context
* A wrapper that contains the code that is running, in order to manage the lexical environment and the scope of the executable code.
### `undefined`
* A special value within JS given to variables that have been declared, but has yet to be defined.  `undefined` is not a string, but instead a unique special value that acts as a place holder in memory for a future value.   
### Name / Value Pair (Key/Value)
* A name that maps to a unique value.  The name may be defined more than once, but can only have one value in any given execution context.  That value can be defined to hold further sets of name / value pairs
### Object
* A collection of properties with name / value pairs or methods which are declared functions within the object's collection.  An object can also have a collection that is another object.    
### Object Literal
* When the JS Engine is parsing through your code and comes upon `{}` outside of a function or conditional, the JS Engine assumes that a new object is being created.  Object literal syntax allows the programmer to create an object on the fly with `{}` whenever wanted.  
### Namespace
* A container for variables and functions to be held so that separate variables and functions, possibly with the same name, do not cause conflict when called.  JS doesn't have a specialty namespace syntax, but objects can be used to accomplish this task.  
### JSON (JavaScript Object Notation)  
* A string of data that is almost identical to object literal syntax except all property names have to be wrapped in quotes.  JSON is a subset of object literal syntax, meaning that all object literal cannot be used as JSON, but all JSON can be used as object literal.  
### Hoisting
* The JS Engine hoists important values to the top of the script when code is run.  Upon execution, the parser identifies variables and functions, creating memory for those values at the top of the compiled code.  This hoisting happens in two steps during the execution context.  In the creation phase of the execution context, all variables are hoisted pointing to `undefined` as the value, where as functions are hoisted pointing to the function code. In the execution phase all variables are defined and code is executed.
### Synchronous
* Code is read and executed in the order that is parsed.  The JS Engine runs synchronously through code.     
### Asynchronous
* Code that does not run in a particular order, but instead is executed to completion based on a trigger event.  The browser run asynchronously through events as they are added to the even queue passed by the JS Engine.           
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
### Function Invocation
* Running a function, or calling a function, which causes a new execution context to be created and executed.
### Scope Chain
* The execution process that looks to the execution stack, the scope of variables, and the scope of functions to correctly assign values based on lexical scope.  This means that a function looks for value within its lexical scope, and if nothing is found the function looks down the chain at its parent's lexical scope for the value.  Scopes are chained together based on relationship, allowing children access to outer value. If variables, at different levels of the code, have identical declarations with different values, functions look to evaluate based on the defined value of the variable closest in scope.
```JavaScript
  function a() {
    function b() {
      console.log(variableOne);
    }
    var variableOne = 2;
    b ();
  }
  var variableOne = 1;
  a();
  //"2" is the output of function "a" because function "b"
  //evaluates "variableOne" at the same scoped level that
  //it is invoked. Function "b" completes its code and pops
  //off the execution stack before it reaches the outer
  //"variableOne" equal to "1".
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
