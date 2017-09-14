# The Weird Parts of JavaScript
## Definitions
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
### "this"
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
* If an inner method is trying to access an outer function's `this`, then `this` must be saved to a new variable before the inner method is invoked.  This means that a new variable, `that`, points to the properties and methods of the outer object so that the values can be accessed within the closure, otherwise `this` used within a closure returns `undefined`.  
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
  //"this" works only one level into an invoking object, once a closure is invoked "that" must be used to refer to the original invoking object.
```   
