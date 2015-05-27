##5. Higher-Order functions##

**Concept d'abstraction** : cache les détails d'implémentation, limite bugs...

*Example : forEach, standard method on arrays*
```
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

*Example : filter, standard method on arrays*
```
console.log ( ancestryArray.filter( function( person ) {
  return person.father == " Carel Haverbeke ";
}) ) ;
// → [{ name : " Carolus Haverbeke ", ...}]
```

*Example : map*

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


*Example : reduce*

```
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

```
console.log ( ancestry.reduce ( function ( min , cur ) {
  if ( cur.born < min.born ) return cur ;
  else return min ;
}) ) ;
// → { name : " Pauwels van Haverbeke " , born : 1535 , ...}
```

###Functions that create new functions :###

```
function greaterThan ( n ) {
  return function ( m ) { return m > n; };
}

var greaterThan10 = greaterThan (10) ;
console.log ( greaterThan10 (11) ) ;
// → true
```


###And you can have functions that change other functions.###

```
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

#### Apply ####

Method that can be used to call funtions with an array specifying their arguments.
You pass it an array (or array-like object) of arguments, and it will call
the function with those arguments.

```
function transparentWrapping ( f ) {
  return function () {
    return f.apply ( null , arguments );
  };
}
```

The function it returns passes all of the given arguments, and only those
arguments, to f.It does this by passing its own arguments object to apply .
The first argument to apply , for which we are passing null here, can be
used to simulate a method call.

#### Bind ####

Method which is used to create a partially applied version of the function.
The bind method, which all functions have, creates a new function that
will call the original function but with some of the arguments already
fixed.


```
var theSet = [" Carel Haverbeke ", " Maria van Brussel ", " Donald Duck "];
function isInSet ( set , person ) {
  return set.indexOf ( person.name ) > -1;
}
```

To call filter in order to collect those person objects whose names are in a
specific set, we can either write a function expression that makes a call
to isInSet with our set as its first argument or partially apply the isInSet
function. The call to bind returns a function that will call isInSet with theSet as first argument, followed by any remaining arguments given to the bound function.

```
console.log ( ancestry.filter ( function ( person ) {
  return isInSet ( theSet , person );
}) ) ;
// → [{ name : " Maria van Brussel ", ...}, { name : " Carel Haverbeke " , ...}]

console.log ( ancestry.filter ( isInSet.bind ( null , theSet )));
// → ... same result
```