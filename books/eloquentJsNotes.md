* [5. Higher-Order functions](#chap5)
  * forEach, filter, map, reduce
  * apply, bind
* [6. The Secret Life of Objects](#chap6)
  * call, prototypes, constructors, defineProperty, hasOwnProperty
  * Interface and Polymorphism, inheritance, instanceOf
* [8. Bugs and error handling](#chap8)
* [9. Regular expressions](#chap9)
* [10. Modules](#chap10)

___

##<a name="chap5">5. Higher-Order functions##

**Concept d'abstraction** : cache les détails d'implémentation, limite bugs...

*Example : __forEach__, standard method on arrays*

```javascript
function gatherCorrelations(journal ) {
  var phis = {};
  journal.forEach(function(entry ) {
    entry.events.forEach(function(event ) {
      if(!(event in phis ) )
        phis[event] = phi(tableFor(event , journal ));
      }) ;
    }) ;
  return phis ;
}
```

*Example : __filter__, standard method on arrays*

```javascript
console.log(ancestryArray.filter(function(person ) {
  return person.father == " Carel Haverbeke ";
}) ) ;
// → [{ name : " Carolus Haverbeke ", ...}]
```

*Example : __map__*

```javascript
function map(array , transform ) {
  var mapped = [];
  for(var i = 0; i < array.length ; i ++)
    mapped.push(transform(array [ i ]) );
  return mapped ;
}

var overNinety = ancestry.filter(function(person ) {
  return person.died - person.born > 90;
}) ;

console.log(map(overNinety , function(person ) {
  return person.name ;
}) ) ;
// → [" Clara Aernoudts " , " Emile Haverbeke ", " Maria Haverbeke "]
```

*Example : __reduce__*

```javascript
function reduce(array , combine , start ) {
  var current = start ;
  for(var i = 0; i < array.length ; i ++)
    current = combine(current , array [i ]) ;
  return current ;
}

console.log(reduce([1 , 2 , 3 , 4] , function(a , b) {
  return a + b;
}, 0) ) ;
// → 10
```

If your array contains at least one element, you are allowed to leave off the start argument. The method will take the first element of the array as its start value and start reducing at the second element.

```javascript
console.log(ancestry.reduce(function(min , cur ) {
  if(cur.born < min.born ) return cur ;
  else return min ;
}) ) ;
// → { name : " Pauwels van Haverbeke " , born : 1535 , ...}
```

###Functions that create new functions :###

```javascript
function greaterThan(n ) {
  return function(m ) { return m > n; };
}

var greaterThan10 = greaterThan(10) ;
console.log(greaterThan10(11) ) ;
// → true
```


###Functions that change other functions###

```javascript
function noisy(f ) {
  return function(arg ) {
    console.log(" calling with ", arg ) ;
    var val = f(arg ) ;
    console.log(" called with " , arg , "- got ", val );
    return val ;
  };
}

noisy(Boolean )(0) ;
// → calling with 0
// → called with 0 - got false
```

####Apply####

Method that can be used to call funtions with an array specifying their arguments.
You pass it an array(or array-like object) of arguments, and it will call
the function with those arguments.

```javascript
function transparentWrapping(f ) {
  return function() {
    //  Null as a first argument is used to give a value to this.
    return f.apply(null , arguments );
  };
}
```

The function it returns passes all of the given arguments, and only those
arguments, to f.It does this by passing its own arguments object to apply .
The first argument to apply , for which we are passing null here, can be
used to simulate a method call.

####Bind####

Method which is used to create a partially applied version of the function.
The bind method, which all functions have, creates a new function that
will call the original function but with some of the arguments already
fixed.


```javascript
var theSet = [" Carel Haverbeke ", " Maria van Brussel ", " Donald Duck "];
function isInSet(set , person ) {
  return set.indexOf(person.name ) > -1;
}
```

To call filter in order to collect those person objects whose names are in a specific set, we can either write a function expression that makes a call
to isInSet with our set as its first argument or partially apply the isInSet
function. The call to bind returns a function that will call isInSet with theSet as first argument, followed by any remaining arguments given to the bound function.

```javascript
console.log(ancestry.filter(function(person ) {
  return isInSet(theSet , person );
}) ) ;
// → [{ name : " Maria van Brussel ", ...}, { name : " Carel Haverbeke " , ...}]

console.log(ancestry.filter(isInSet.bind(null , theSet )));
//  Null as a first argument is used to give a value to this.
// → ... same result
```

##<a name="chap6">6. The Secret Life of Objects##

####Call####

There is a method similar to apply, called call. It also calls the function it is a method of but takes its arguments normally, rather than as an array. Like apply and bind, call can be passed a specific this value.

```javascript
speak.apply(fatRabbit, ["Burp!"]);
// → The fat rabbit says 'Burp!'
speak.call({type: "old"}, "Oh my.");
// → The old rabbit says 'Oh my.'
```

####Prototypes####

Objects have prototypes, which are other objects, and will act as if they have properties they don’t have as long as the prototype has that property. Simple objects have Object.prototype as their prototype.

**`Object.getPrototypeOf`**

The `Object.getPrototypeOf` function obviously returns the prototype of an object.

```javascript
console.log(Object.getPrototypeOf({}) ==
            Object.prototype);
// → true
console.log(Object.getPrototypeOf(Object.prototype));
// → null
console.log(Object.getPrototypeOf(isNaN) ==
            Function.prototype);
// → true
console.log(Object.getPrototypeOf([]) ==
            Array.prototype);
// → true
```

**`Object.create`**

You can use `Object.create` to create an object with a specific prototype.

```javascript
var protoRabbit = {
  speak: function(line) {
    console.log("The " + this.type + " rabbit says '" +
                line + "'");
  }
};
var killerRabbit = Object.create(protoRabbit);
killerRabbit.type = "killer";
killerRabbit.speak("SKREEEE!");
// → The killer rabbit says 'SKREEEE!'
```

####Constructors####

Constructors, which are functions whose names usually start with a capital letter, can be used with the new operator to create new objects. The new object’s prototype will be the object found in the prototype property of the constructor function. You can make good use of this by putting the properties that all values of a given type share into their prototype.

```javascript
function Rabbit(type) {
  this.type = type;
}

var killerRabbit = new Rabbit("killer");
var blackRabbit = new Rabbit("black");
console.log(blackRabbit.type);
// → black
console.log(Object.getPrototypeOf(blackRabbit));
// → Rabbit{}
console.log(Object.getPrototypeOf(Rabbit));
// → function Empty(){…}
console.log(Rabbit.prototype);
// → Rabbit{}
```

Constructors(in fact, all functions) automatically get a property named prototype, which by default holds a plain, empty object that derives from Object.prototype.

So to add a speak method to rabbits created with the Rabbit constructor, we can simply do this:

```javascript
Rabbit.prototype.speak = function(line) {
  console.log("The " + this.type + " rabbit says '" +  line + "'");
```

####Prototypes, enumerable, defineProperty####

JavaScript distinguishes between **enumerable** and **nonenumerable properties**. All properties that we create by simply assigning to them are enumerable. The standard properties in Object.prototype are all nonenumerable.

**Object.defineProperty**

It is possible to define our own nonenumerable properties by using the `Object.defineProperty` function, which allows us to control the type of property we are creating.

```javascript
Object.defineProperty(Object.prototype, "hiddenNonsense",
                      {enumerable: false, value: "hi"});
```

**hasOwnProperty**

The `hasOwnProperty` method tells us whether the object itself has the property, without looking at its prototypes.

When you are worried that someone(some other code you loaded into your program) might have messed with the base object prototype, I recommend you write your for/in loops like this:

```javascript
for(var name in map) {
  if(map.hasOwnProperty(name)) {
    // ... this is an own property
  }
}
```

####Prototype-less Objects####

The `Object.create` function allows us to create an object with a specific prototype. You are allowed to pass `null` as the prototype to create a fresh object with no prototype.

```javascript
var map = Object.create(null);
map["pizza"] = 0.069;
console.log("toString" in map);
// → false
console.log("pizza" in map);
// → true
```

###Interface and Polymorphism###

One useful thing to do with objects is to specify an **interface** for them and tell everybody that they are supposed to talk to your object only through that interface. The rest of the details that make up your object are now encapsulated, hidden behind the interface.

Once you are talking in terms of interfaces, who says that only one kind of object may implement this interface? Having different objects expose the same interface and then writing code that works on any object with the interface is called **polymorphism**.

####Getters and Setters####

In object literal, the `get` or `set` notation for properties allows you to specify a function to be run when the property is read or written.

```javascript
var pile = {
  elements: ["eggshell", "orange peel", "worm"],
  get height() {
    return this.elements.length;
  },
  set height(value) {
    console.log("Ignoring attempt to set height to", value);
  }
};

console.log(pile.height);
// → 3
pile.height = 100;
// → Ignoring attempt to set height to 100
```

You can also add such a property to an existing object, for example a prototype, using the `Object.defineProperty` function(which we previously used to create nonenumerable properties).

```javascript
Object.defineProperty(TextCell.prototype, "heightProp", {
  get: function() { return this.text.length; }
});

var cell = new TextCell("no\nway");
console.log(cell.heightProp);
// → 2
cell.heightProp = 100;
console.log(cell.heightProp);
// → 2
```

####Inheritance####

```javascript
function RTextCell(text) {
  TextCell.call(this, text);
}
RTextCell.prototype = Object.create(TextCell.prototype);
RTextCell.prototype.draw = function(width, height) {
  var result = [];
  for(var i = 0; i < height; i++) {
    var line = this.text[i] || "";
    result.push(repeat(" ", width - line.length) + line);
  }
  return result;
};
```

You can have polymorphism without inheritance. A preferable way to extend types is through composition, such as how UnderlinedCell builds on another cell object by simply storing it in a property and forwarding method calls to it in its own methods.

####Instanceof####

The instanceof operator can, given an object and a constructor, tell you whether that object is an instance of that constructor.

```javascript
console.log(new RTextCell("A") instanceof RTextCell);
// → true
console.log(new RTextCell("A") instanceof TextCell);
// → true
console.log(new TextCell("A") instanceof RTextCell);
// → false
console.log([1] instanceof Array);
// → true
```

___

##<a name="chap8">8. Bugs and error handling##

###Strict mode###

You can put `use strict` at the top of a file or a function body.
- when you forget to put var in front of your variable, an error is reported(this doesn’t work when the variable in question already exists as a global variable, but only when assigning to it would have created it.)
- the this binding holds the value undefined in functions that are not called as methods.
- disallows giving a function multiple parameters with the same name and removes certain problematic language features entirely

###Exceptions###

```javascript
function promptDirection(question) {
  var result = prompt(question, "");
  if(result.toLowerCase() == "left") return "L";
  if(result.toLowerCase() == "right") return "R";
  throw new Error("Invalid direction: " + result);
}

function look() {
  if(promptDirection("Which way?") == "L")
    return "a house";
  else
    return "two angry bears";
}

try {
  console.log("You see", look());
} catch(error) {
  console.log("Something went wrong: " + error);
}
```

###Cleaning code when catching exceptions###

```javascript
function withContext(newContext, body) {
  var oldContext = context;
  context = newContext;
  try {
    return body();
  } finally {
    context = oldContext;
  }
}
```

###Custom exceptions###

```javascript
function InputError(message) {
  this.message = message;
  this.stack =(new Error()).stack;
}
InputError.prototype = Object.create(Error.prototype);
InputError.prototype.name = "InputError";

for(;;) {
  try {
    var dir = promptDirection("Where?");
    console.log("You chose ", dir);
    break;
  } catch(e) {
    if(e instanceof InputError)
      console.log("Not a valid direction. Try again.");
    else
      throw e;
  }
}
```

The prototype is made to derive from Error.prototype so that instanceof Error will also return true for InputError objects. It’s also given a name property since the standard error types(Error, SyntaxError, ReferenceError, and so on) also have such a property.

The assignment to the stack property tries to give this object a somewhat useful stack trace, on platforms that support it, by creating a regular error object and then using that object’s stack property as its own.

###Assertions###

```javascript
function AssertionFailed(message) {
  this.message = message;
}
AssertionFailed.prototype = Object.create(Error.prototype);

function assert(test, message) {
  if(!test)
    throw new AssertionFailed(message);
}

function lastElement(array) {
  assert(array.length > 0, "empty array in lastElement");
  return array[array.length - 1];
}
```

___

##<a name="chap9">9. Regular Expressions##

###Généralités###

```javascript
var re1 = new RegExp("abc");
var re2 = /abc/;
```

Some characters, such as question marks and plus signs, have special meanings in regular expressions and must be preceded by a backslash if they are meant to represent the character itself. When in doubt, just put a backslash before any character that is not a letter, number, or whitespace.

```javascript
var eighteenPlus = /eighteen\+/;
```

Within square brackets, a dash(`-`) between two characters can be used to indicate a range of characters, where the ordering is determined by the character’s Unicode number.


`\d`  Any digit character
`\w`  An alphanumeric character(“word character”)
`\s`  Any whitespace character(space, tab, newline, and similar)
`\D`  A character that is not a digit
`\W`  A nonalphanumeric character
`\S`  A nonwhitespace character
`.`   Any character except for newline

`[\d.]` means *any* digit or a period character.

To *invert* a set of characters—that is, to express that you want to match any character except the ones in the set—you can write a caret(`^`) character after the opening bracket.

###Matching###

```javascript
console.log(/abc/.test("zabcde"));
// → true
console.log(/abc/.test("abxde"));
// → false
```

Matching any number :

```javascript
console.log(/[0123456789]/.test("in 1992"));
// → true
console.log(/[0-9]/.test("in 1992"));
// → true
```

Match a date and time format like 30-01-2003 15:20 with the following expression:

```javascript
var dateTime = /\d\d-\d\d-\d\d\d\d \d\d:\d\d/;
console.log(dateTime.test("30-01-2003 15:20"));
// → true
console.log(dateTime.test("30-jan-2003 15:20"));
// → false
```

```javascript
var notBinary = /[^01]/;
console.log(notBinary.test("1100100010100110"));
// → false
console.log(notBinary.test("1100100010200110"));
// → true
```

###Repeating parts of a pattern###

- `+` : the element *may be repeated more than once*.
- `*` : similar meaning but also *allows the pattern to match zero times*.
-  `?` : *makes a part of a pattern “optional”*(it may occur zero or one time).
- `{4}` : requires an element to occur *exactly four times*
- `{,5}` : means *zero to five times*.
- `{5,}` : means *five or more times*.

```javascript
console.log(/'\d+'/.test("'123'"));
// → true
console.log(/'\d+'/.test("''"));
// → false
console.log(/'\d*'/.test("'123'"));
// → true
console.log(/'\d*'/.test("''"));
// → true

var neighbor = /neighbou?r/;
console.log(neighbor.test("neighbour"));
// → true
console.log(neighbor.test("neighbor"));
// → true

var dateTime = /\d{1,2}-\d{1,2}-\d{4} \d{1,2}:\d{2}/;
console.log(dateTime.test("30-1-2003 8:45"));
// → true
```

###Grouping subexpressions###

- `()` : A part of a regular expression that is enclosed in parentheses *counts as a single element* as far as the operators following it are concerned
- `i` : makes a regular expression *case insensitive*


```javascript
var cartoonCrying = /boo+(hoo+)+/i;
console.log(cartoonCrying.test("Boohoooohoohooo"));
// → true
// The first and second + characters apply only to the second o in boo and hoo, respectively. The third + applies to the whole group(hoo+), matching one or more sequences like that.
```

###Matches and group###

- `exec` : will return null if no match was found and return an object with information about the match otherwise.
- `match` : quite the same as `exec`, for strings

```javascript
var match = /\d+/.exec("one two 100");
console.log(match);
// → ["100"]
console.log(match.index);
// → 8

console.log("one two 100".match(/\d+/));
// → ["100"]
```

##<a name="chap10">10. Modules##

###Using functions as namespaces###

*Functions are the only things in JavaScript that create a new scope*. So if we want our modules to have their own scope, we will have to base them on functions.

From :

```javascript
var names = ["Sunday", "Monday", "Tuesday", "Wednesday",
             "Thursday", "Friday", "Saturday"];
function dayName(number) {
  return names[number];
}

console.log(dayName(1));
// → Monday
```

To :

```javascript
var dayName = function() {
  var names = ["Sunday", "Monday", "Tuesday", "Wednesday",
               "Thursday", "Friday", "Saturday"];
  return function(number) {
    return names[number];
  };
}();
```

###Objects as interfaces###

If we want to add another function, we can’t simply return the function anymore but must wrap the two functions in an object.

```javascript
var weekDay = function() {
  var names = [" Sunday " , " Monday " , " Tuesday ", " Wednesday ", " Thursday " , " Friday " , " Saturday "];
  return {
    name : function(number ) { return names [ number ]; },
    number : function(name ) { return names.indexOf(name ); }
  };
}() ;
console.log(weekDay.name(weekDay.number(" Sunday ") ));
// → Sunday
```

For bigger modules, a convenient alternative is to declare an object (conventionally named {exports{}) and add properties to that whenever we are defining something that needs to be exported.

```javascript
(function(exports) {
  var names = ["Sunday", "Monday", "Tuesday", "Wednesday",
               "Thursday", "Friday", "Saturday"];

  exports.name = function(number) {
    return names[number];
  };
  exports.number = function(name) {
    return names.indexOf(name);
  };
})(this.weekDay = {});

console.log(weekDay.name(weekDay.number("Saturday")));
// → Saturday
```

###Home made require###

**Evaluating data as code**

A good way of interpreting data as code is to use the `Function` constructor. This takes **two arguments**: a string containing a **comma-separated list of argument names** and a string containing the **function’s body**.

```javascript
var plusOne = new Function("n", "return n + 1;");
console.log(plusOne(4));
// → 5
```

**Minimal immplementation**

```javascript
function require(name) {
  var code = new Function("exports", readFile(name));
  var exports = {};
  code(exports);
  return exports;
}

console.log(require("weekDay").name(1));
// → Monday
```

Problems:
-  it will **load and run a module every time it is required** ->  storing the modules that have already been loaded in an object and simply returning the existing value when one is loaded multiple times.
-   it is not possible for a module to directly **export a value other than the exports object**, such as a function -> provide modules with another variable, module, which is an object that has a property exports

**CommonJS modules**

```javascript
function require(name) {
  var code = new Function("exports", readFile(name));
  var exports = {};
  code(exports);
  return exports;
}

console.log(require("weekDay").name(1));
// → Monday
```

**Asynchronous Module Definition (AMD)**

**Browserify** (alternate solution) : program to run on your code before you serve it on a web page. Will look for calls to `require`, resolve all dependencies, and gather the needed code into a single big file. The website itself can simply load this file to get all the modules it needs.

**AMD** : wrap the code that makes up your module in a function so that the module loader can first load its dependencies in the background and then call the function, initializing the module, when the dependencies have been loaded.


`define` : takes first an **array of module names** and then a **function that takes one argument for each dependency**. It will load the dependencies (if they haven’t already been loaded) in the background, allowing the page to continue working while the files are being fetched. Once all dependencies are loaded, define will call the function it was given, with the interfaces of those dependencies as arguments.

*example of module loaded via define :*
The modules must contain a **call to define**. The **value used as their interface** is whatever was returned by the function passed to define.

```javascript
define([], function() {
  var names = ["Sunday", "Monday", "Tuesday", "Wednesday",
               "Thursday", "Friday", "Saturday"];
  return {
    name: function(number) { return names[number]; },
    number: function(name) { return names.indexOf(name); }
  };
});
```

*objects that describe the state of modules :*

```javascript
var defineCache = Object.create(null);
var currentMod = null;
```

*getModule function :*

 When given a name, will return such an object  that describe the state of modules, telling us whether they are available yet and providing their interface when they are, and ensure that the module is scheduled to be loaded. It uses a cache object to avoid loading the same module twice.