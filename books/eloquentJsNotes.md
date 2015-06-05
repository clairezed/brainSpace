* [5. Higher-Order functions](#chap5)
  * forEach, filter, map, reduce
  * apply, bind
* [6. The Secret Life of Objects](#chap6)
  * call, prototypes, constructors, defineProperty, hasOwnProperty
  * Interface and Polymorphism, inheritance, instanceOf

___

##<a name="chap5">5. Higher-Order functions##

**Concept d'abstraction** : cache les détails d'implémentation, limite bugs...

*Example : __forEach__, standard method on arrays*

```javascript
function gatherCorrelations( journal ) {
  var phis = {};
  journal.forEach( function( entry ) {
    entry.events.forEach ( function( event ) {
      if (!( event in phis ) )
        phis[event] = phi( tableFor( event , journal ));
      }) ;
    }) ;
  return phis ;
}
```

*Example : __filter__, standard method on arrays*

```javascript
console.log ( ancestryArray.filter( function( person ) {
  return person.father == " Carel Haverbeke ";
}) ) ;
// → [{ name : " Carolus Haverbeke ", ...}]
```

*Example : __map__*

```javascript
function map ( array , transform ) {
  var mapped = [];
  for ( var i = 0; i < array.length ; i ++)
    mapped.push ( transform ( array [ i ]) );
  return mapped ;
}

var overNinety = ancestry.filter ( function ( person ) {
  return person.died - person.born > 90;
}) ;

console.log ( map ( overNinety , function ( person ) {
  return person.name ;
}) ) ;
// → [" Clara Aernoudts " , " Emile Haverbeke ", " Maria Haverbeke "]
```

*Example : __reduce__*

```javascript
function reduce ( array , combine , start ) {
  var current = start ;
  for ( var i = 0; i < array.length ; i ++)
    current = combine ( current , array [i ]) ;
  return current ;
}

console.log ( reduce ([1 , 2 , 3 , 4] , function (a , b) {
  return a + b;
}, 0) ) ;
// → 10
```

If your array contains at least one element, you are allowed to leave off the start argument. The method will take the first element of the array as its start value and start reducing at the second element.

```javascript
console.log ( ancestry.reduce ( function ( min , cur ) {
  if ( cur.born < min.born ) return cur ;
  else return min ;
}) ) ;
// → { name : " Pauwels van Haverbeke " , born : 1535 , ...}
```

### Functions that create new functions : ###

```javascript
function greaterThan ( n ) {
  return function ( m ) { return m > n; };
}

var greaterThan10 = greaterThan (10) ;
console.log ( greaterThan10 (11) ) ;
// → true
```


###Functions that change other functions###

```javascript
function noisy ( f ) {
  return function ( arg ) {
    console.log (" calling with ", arg ) ;
    var val = f ( arg ) ;
    console.log (" called with " , arg , "- got ", val );
    return val ;
  };
}

noisy ( Boolean ) (0) ;
// → calling with 0
// → called with 0 - got false
```

####Apply####

Method that can be used to call funtions with an array specifying their arguments.
You pass it an array (or array-like object) of arguments, and it will call
the function with those arguments.

```javascript
function transparentWrapping ( f ) {
  return function () {
    //  Null as a first argument is used to give a value to this.
    return f.apply ( null , arguments );
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
function isInSet ( set , person ) {
  return set.indexOf ( person.name ) > -1;
}
```

To call filter in order to collect those person objects whose names are in a specific set, we can either write a function expression that makes a call
to isInSet with our set as its first argument or partially apply the isInSet
function. The call to bind returns a function that will call isInSet with theSet as first argument, followed by any remaining arguments given to the bound function.

```javascript
console.log ( ancestry.filter ( function ( person ) {
  return isInSet ( theSet , person );
}) ) ;
// → [{ name : " Maria van Brussel ", ...}, { name : " Carel Haverbeke " , ...}]

console.log ( ancestry.filter ( isInSet.bind ( null , theSet )));
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

Constructors (in fact, all functions) automatically get a property named prototype, which by default holds a plain, empty object that derives from Object.prototype.

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

When you are worried that someone (some other code you loaded into your program) might have messed with the base object prototype, I recommend you write your for/in loops like this:

```javascript
for (var name in map) {
  if (map.hasOwnProperty(name)) {
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

You can also add such a property to an existing object, for example a prototype, using the `Object.defineProperty` function (which we previously used to create nonenumerable properties).

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
  for (var i = 0; i < height; i++) {
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
