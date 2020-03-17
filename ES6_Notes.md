# JavaScript ES6 Notes

### **Arrow functions**

Arrow functions behave differently than traditional JavaScript functions in a number of important wats:

* **No this, super, arguments, and new.target bindings.**
* **Cannot be called with `new`** *Arrow functions fo not have a `[[Construct]]` method and therefore cannot be used as constructors. Arrow functions throw an error when used with new*.
* **No prototype** *The `prototype` property of an arrow function doesn't exist.*
* **Can't change `this`**
* **No arguments object** *(You must rely on named and res parameters ot access function arguments)*
* **No duplicate named parameters**

If you are passing in more than one argument, you must include parentheses around those arguments, like this:

```javascript
let sum = (num1, num2) => num1 + num2;
```

If there are no arguments to the function, you must include an empty set of parentheses in the declaration, as follows.

````javascript
let getName = () => "Nicholas";
````

When you want to provide a more traditional function body, perhaps consisting of more than one expression, you need to wrap the function body in curly braces and explicitly define a return value, as in this version of sum():

````javascript
let sum = (a, b) => {
  return a + b;
};
````

An arrow function that wants to return an object literal outside a function body must wrap the literal in parentheses. For example:

````javascript
let getTempItem = id => ({id: id, name: "Temp"});
````

> Wrapping the object literal in parentheses signals that the curly braces are an object literal instead of the function body

**Creating Immediately Invoked Funtion Expressions (IIFE)**

````javascript
// ES5 IIFE
let person = function(name) {
  return {
    getName: function() {
      return true;
    }
  }
}('Nocholas');
````

````javascript
// ES6 IIFE
let person = ((name) => {
  return {
    getName: function() {
      return name;
    }
  };
})("Nicholas");
````

**Rest Parameters**

A *rest* parameter is indicated by three dots (...) preceding a named parameter. That named parameter becomes an Array containing the rest of the parameters passed to the function.

```javascript
function pick(object, ...keys) {
  let result = Object.create(null);

  for (let i = 0, len = keys.length; i < len; i++) {
    result[keys[i]] = object[keys[i]];
  }

  return result;
}
```

**Rest Parameter Restrictions**

Rest parameters have two restrictions. The first restriction is that there can be only on e rest parameter, and the rest parameter must be last. The second restriction is that rest parameters cannot be used in an object literal setter. That means this code would also cause a syntax error.

````javascript
// Example of use
function checkArgs(...args) {
  console.log(args.length);
  console.log(arguments.length);
  console.log(args[0], arguments[0]);
  console.log(args[1], arguments[1]);
}

checkArgs("a", "b");
````

**Spread Operator**

The spread operator allows you  to specify an array that should be split and passed in as separate arguments to a function.

```javascript
let values = [25, 50, 75, 100];

// equivalent to
// console.log(Math.max(25, 50, 75, 100));
console.log(Math.max(...values)); // 100
```

**Object Destructuring**

Object restructuring syntax uses an object literal on the left side of an assignment operation. For example:

````javascript
let node = {
  type: "Identifier",
  name: "foo"
};

let { type, name } = node;

console.log(type);
console.log(name);
````

**Default values**

You can optionally define a default value to use when a specified property doesn't exist. To do so, insert an equal sign `(=)` after the property name and specify the default value, like this.

````javascript
let node = {
  type: "Identifier",
  name: "foo"
};

let { type, name, value = true } = node;

console.log(type);
console.log(name);
console.log(value);
````

The default value is only used if the property is missing on node or has a value of `undefined`.

**Assigning to Different Local Variable Name**

````javascript
let node = {
  type: "Identifier",
  name: "foo"
};

let { type: localType, name: localName } = node;

console.log('local', localType);
console.log('local', localName);
````

This code uses destructuring assignments to declare the `localType` and `localName` variables, which contain the values from the `node.type` and `node.name` properties, respectively.

You can add default values when you're using a different variable name, as well. The equal sign and default value are still placed after the local variable name. For example:

````javascript
let node = {
  type: "Identifier"
};

let { type: localType, name: localName = "bar" } = node;

console.log(localType);
console.log(localName);
````

**Nested Object Destructuring**

````javascript
let node = {
  type: "Identifier",
  name: "foo",
  loc: {
    start: {
      line: 1,
      column: 1
    },
    end: {
      line: 1,
      column: 4
    }
  }
};

let { loc: { start }} = node;
// A curly brace after the colon indicates that the destination is nested another level into the object.

console.log(start.line);
console.log(start.column);
````

Use a different name for the local variable.

```javascript
let { loc: { start: localStart }} = node;

console.log(localStart.line);
console.log(localStart.column);
```

**Array Destructuring**

The destructuring operates on positions within an array rather than the named properties that are available in objects.

````javascript
let colors = ['red', 'green', 'blue'];

let [ firstColor, secondColor ] = colors;

console.log(firstColor);
console.log(secondColor);
````

You can omit items in the destructuring pattern and only provide variable name for the items your're interested in.

````javascript
let colors = ['red', 'green', 'blue'];

// Destructuring assignment
let [, , thirdColor ] = colors;

console.log(thirdColor);
````

**Destructuring assignment**

````javascript
// Swapping variables in ECMAScript 6
let a = 1, b = 2;

[a, b] = [b, a];
 
console.log(a); // 2
console.log(b); // 1
````

**Default Values**

````javascript
let colors = ['red'];

let [firstColor, secondColor = "green"] = colors;

  console.log(firstColor); // 'red'
console.log(secondColor); // 'green'
````

**Nested Array Destructuring**

````javascript
let colors = ['red', ['green', 'lightgreen'], 'blue'];

let [firstColor, [ secondColor ] ] = colors;

console.log(firstColor);
console.log(secondColor);
````

**Rest Items**

Rest items use the`...` syntax to assign the remaining items in an array to a articular variable.

````javascript
let colors = ['red', 'green', 'blue'];

let [firstColor, ...restColors ] = colors;

console.log(firstColor);
console.log(restColors.length);
console.log(restColors[0]);
console.log(restColors[1]);
````

**Cloning an array in ECMAScript 6**

In this example, rest items are used to copy values from the `colors` array into the `clonedColors`

````javascript
let colors = ['red', 'green', 'blue'];
let [...clonedColors ] = colors;

console.log(clonedColors);
````

> Rest items must be the last entry in the restructured array and cannot be followed by a comma. Including a comma after rest items is a syntax error.

**Mixed Destructuring**

````javascript
let node = {
  type: "Identifier",
  name: "foo",
  loc: {
    start: {
      line: 1,
      column: 1
    },
    end: {
      line: 1,
      column: 4
    }
  },
  range: [0, 3]
};

let { loc: { start }, range: [ startIndex ] } = node;

console.log(start.line);
console.log(start.column);
console.log(startIndex);
````

**Sets in ECMAScript 6**

ECMAScript 6 adds a Set type that is an ordered list of values without duplicates. Sets allow fast access to the data they contain, adding a more efficient manner of tracking discrete values.

**Creating Sets and Adding Items**

Sets don't coerce values to determine whether they're the same. The comparison uses the `Object.is()` method to determine if two values are the same.

````javascript
let set = new Set();

set.add(5);
set.add("5");

console.log(set.size);
````

You can also add multiple objects to the set, and those objects will remain distinct:

````javascript
let set = new Set(),
    key1 = {},
    key2 = {};

set.add(key1);
set.add(key2);

console.log(set.size);
````

Because `key1` and `key2` are not converted to strings, they count as two unique items in the set. If the method is called more than once with the same value, all calls after the first one are effectively ignored.

````javascript
let set = new Set();

set.add(5);
set.add("5");
set.add(5); // duplicate - this is ignored

console.log(set.size); // 2
````

You can also initialize a set using an array, and the Set constructor will ensure that only unique values are used. For instance:

````javascript
let set = new Set([1, 2, 3, 4, 5, 5, 5, 5]);

console.log(set.size); // 5
````

You can test which values are in a set using the `has()` method, like this:

````javascript
let set = new Set();

set.add(5);
set.add("5");

console.log(set.has(5)); // true
console.log(set.has(6)); // false
````

**Removing Items**

It's also possible ro remove items from a set. You can remove a single item by using the `delte()` method, or you can remove all items from the set by calling the `clear()` method. This code shows both in action:

````javascript
let set = new Set();

set.add(5);
set.add('5');

console.log(set.has(5)); // true

set.delete(5);

console.log(set.has(5)); // false
console.log(set.size); // 1

set.clear();

console.log(set.has('5')); // false
console.log(set.size); // 0
````

**The `forEach()` Method for Sets**

The `forEach()` method is passed a callback function that accepts three arguments:

* The value from the next position in the set
* The same value as the first argument
* The set from which the value is read

The strange difference between the set version of `forEach()` and the array version is that the first and second arguments to the callback function are the same value in the set version. The first and second argument are always the same in `forEach()` on sets to keep this functionality consistent with the other `forEach()` methods on arrays and maps.

Other than the difference in arguments, using `forEach()` is basically the same for a set as it is for an array. The following code shows the method at work:

````javascript
let set = new Set([1, 2]);

set.forEach( (value, key, ownerSet) => {
  console.log(key, value);
  console.log(ownerSet === set);
});
````

Also the same as arrays, you can pass a this value as the second argument to `forEach()` if you need to use this in your callback function:

````javascript
let set = new Set([1, 2]);

let processor = {
  output(value) {
    console.log(value);
  },
  process(dataSet) {
    dataSet.forEach( value  => this.output(value));
  }
};

processor.process(set);
````

**Converting a Set to an Array**

Converting an array to a set is easy because you can pass the array to the`Set` constructor; converting a set back to an array is also easy if you use the spread operator `(...)`.

`````javascript
let set = new Set([1, 2, 3, 3, 3, 4, 5]);
let array = [...set];

console.log(array);
`````

Here, a set is initially loaded with an array that contains duplicates. The set removes the duplicates, and then the items are placed in into a new array using the spread operator.

This approach is useful when you already have an array and want to create an array without duplicates, as in this example:

````javascript
function eliminateDuplicates(items) {
  return [...new Set(items)];
}

let numbers = [1, 2, 3, 3, 4, 5];
let noDuplicates = eliminateDuplicates(numbers);

console.log(noDuplicates);
````

**Maps in ECMAScript 6**

The map type is an ordered list of ky-value pairs, where the key and value can be any type. You can add items to maps with the key.. You can later retrieve a value by passing the ky to the `get()` method. For example.

````javascript
let map = new Map();
let key1 = {};
let key2 = {};

map.set(key1, {message: 'Hola'});
map.set(key2, 42);

console.log(map.get(key1));
````

**Map Methods**

* **`has(key)`** Determines if the given key exist in the map
* **`delete(key)`** Removes the key and its associated value from the map
* **clear()** Removes all keys and values from the map

Maps also have a size property that indicates how many key-value pairs it contains.

```javascript
let map = new Map();

map.set('name', 'Nicholas');
map.set('age', 25);

console.log(map.size);

console.log(map.has('name'));
console.log(map.get('name'));

map.delete('name');
console.log(map.has('name'));
console.log(map.get('name'));
console.log(map.size);

map.clear();
console.log(map.has('name'));
console.log(map.get('name'));
console.log(map.has('age'));
console.log(map.get('age'));
console.log(map.size);
```

**Map initialization**

Also similar to sets, you can initialize a map with data by passing an array to the Map `constructor`.

```javascript
let map = new Map([['name', 'Nicholas'], ['age', 25]]);

console.log(map.has('name'));
console.log(map.get('name'));
console.log(map.has('age'));
console.log(map.get('age'));
console.log(map.size);
```

**The `forEach()` Method for Maps**

The `forEach()` method for maps is similar to `forEach()` for sets and arrays in that it accepts a callback function that receives three arguments.

* The value from the next position in the map
* The key for that value
* The map from which the value is read

````javascript
let map = new Map([['name', 'Nicholas'], ['age', 25]]);

map.forEach((value, key, ownerMap) => {
    console.log(`${key} ${value}`);
    console.log(ownerMap === map);
});
````
**Iterators**

Iterators are objects with a specific interface designed for iteration. All iterator objects have a `next()` method that returns a result object. The result object has two properties: `value`, which is the next value, and `done`, which is a Boolean that's true when there are no more value to return.

````javascript
function createIterator(items) {
    let i = 0;

    return {
        next: function() {
            let done = (i >= items.length);
            let value = !done ? items[i++] : undefined;

            return {
                done: done,
                value: value
            };
        }
    };
}

const iterator = createIterator([1, 2, 3]);

console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
````

**Generators**

A *generator* is a function that returns an iterator. Generator functions are indicated by an asterisk character (*) after the function keyword and use the new `yield` keyword.


`````javascript
function *createIterator() {
    yield 1;
    yield 2;
    yield 3;
}

// generators are called like regular functions but return an iterator
let iterator = createIterator();

console.log(iterator.next().value);
console.log(iterator.next().value);
console.log(iterator.next().value);
`````

**Generator function expressions**

You can use function expressions to create generators by just including an asterisk (*) between the function keyword and the opening parenthesis.

````javascript
let createIterator = function *(items) {
    for (let i = 0; i < items.length; i++) {
        yield items[i];
    }
};

let iterator = createIterator([1, 2, 3]);

console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
````
In this code, `createIterator()` is a generator function expression instead of a function declaration. The asterisk goes between the `function` keyword and the opening parenthesis because the function expression is anonymoues.

> Creating an arrow function that is also a generator is not posible

**Iterable and for-of Loops**

> An iterable is an object with a Symbol.iterator property. (All iterators created by generators are also iterabvles, because generators assign the Symbol.iterator property by defualt.)

**Creating Iterables**

You can make them iterable by creating a Symbol.iterator property containing a generator.

````javascript
let collection = {
    items: [],
    *[Symbol.iterator]() {
        for (const item of this.items) {
            yield item;
        }
    }
};

collection.items.push(1);
collection.items.push(2);
collection.items.push(3);

for (let x of collection) {
    console.log(x);
}
````
**Build-in Iterators**

You only need to create iterators when the built-in iterators don't serve you purpose, which will most frequently be when defining your own objects or classes.

**Collection Iterators**

ECMAScript 6 has three types of collection objects: arrays, maps and sets. All three have the following built-in iterators to help you navigate their contenet:

* `entries()` Returns an iterator whose values are key-value pairs
* `values()` Returns an iterator whose values are the values of the collection
* `key()` Returns an iterator whose values are the keys contained in the collection

You can retrieve an iterator for a collection by calling one of these methods.

**The `entries()` Iterator**

The `entries()` iterator returns a two-item array each time `next()` is called. The two-item array represents the key and value for each item in the collection. For arrays, the first item is the numeric index; for sets, the first item is also de value (bacause values double as keys in sets); for maps, the first item is the key.

Here are some examples that use the `entries()` iterator:

````javascript
let colors = ['red', 'greed', 'blue'];

for (let entry of colors.entries()) {
    console.log(entry);
}

let tracking = new Set([1234, 5678, 9012]);

for (const entry of tracking.entries()) {
    console.log(entry);
}

let data = new Map();
data.set("title", "Understanding ECMASCRIPT 6");
data.set('format', 'ebook');

for (const entry of data.entries()) {
    console.log(entry);
}

/**
 * [ 0, 'red' ]
 * [ 1, 'greed' ]
 * [ 2, 'blue' ]
 * [ 1234, 1234 ]
 * [ 5678, 5678 ]
 * [ 9012, 9012 ]
 * [ 'title', 'Understanding ECMASCRIPT 6' ]
 * [ 'format', 'ebook' ] 
 * */
````

**The `values()` iterator**

The `values()` iterator simply returns values as the are stored in the collection.

````javascript
let colors = ['red', 'greed', 'blue'];

for (let value of colors) {
    console.log(value);
}

let tracking = new Set([1234, 5678, 9012]);

for (const entry of tracking.values()) {
    console.log(entry);
}

let data = new Map();
data.set("title", "Understanding ECMASCRIPT 6");
data.set('format', 'ebook');

for (const entry of data.values()) {
    console.log(entry);
}

/**
 * red 
 * greed
 * blue
 * 1234
 * 5678
 * 9012
 * Understanding ECMASCRIPT 6
 * ebook
 */
````

**The `keys()` Iterator**

The `keys()` iterator returns each key present in a collection.

````javascript
let colors = ['red', 'greed', 'blue'];

for (let value of colors.keys()) {
  console.log(value);
}

let tracking = new Set([1234, 5678, 9012]);

for (const entry of tracking.keys()) {
  console.log(entry);
}

let data = new Map();
data.set("title", "Understanding ECMASCRIPT 6");
data.set('format', 'ebook');

for (const entry of data.keys()) {
  console.log(entry);
}

/**
 * 0
 * 1
 * 2
 * 1234
 * 5678
 * 9012
 * title
 * format
 ** /
````

**Destructuring and for of loops**

The `for-of` loop in this code uses a destructured array to assign key and value for each entry in the map.

````javascript
let data = new Map();

data.set('title', 'Understanding ECMAScript 6');
data.set('format', 'ebook');

// same as using data.entries()
for (let [key, value] of data) {
  console.log(key + '=' + value);
}
````

**Nodelist Iterators**

You can use `NodeList` in a `for-of` loop or any other place that uses an object's default iterator.

````javascript
const divs = document.getElementByIdByTagName("div");

for (let div of divs) {
    console.log(div.id);
}
````

**The Spread Operator and Nonarray Iterables**

`````javascript
let set = new Set([1, 2, 3, 3, 3, 4, 5]);
let array = [...set];

console.log(array);
`````

This code uses the spread operator inside an array literal to fill in that array with the values from set. The spread operator works on all iterables and uses the dafault iterator to determine which values to include.

You can use the spread operator in an array literal as many times as you want, and you can use it wherever you want to insert multiple items from an iterable. Those items will just appear in order in the new array at the location of the spread operator.

````javascript
let smallNumbers = [1, 2, 3];
let bigNumbers = [100, 101, 102];
let allNumbers = [0, ...smallNumbers, ...bigNumbers];

console.log(allNumbers.length);
console.log(allNumbers);
````
**Passing Arguments to Iterators**

You can passarguments to the iterator through the `next()` method. When you pass an argument to the `next()` method, that argument becomes the value of the yield statement inside a generator.

````javascript
function *createIterator() {
    let first = yield 1;
    let second = yield first + 2;
    yield second + 3;
}

let iterator = createIterator();

console.log(iterator.next());
console.log(iterator.next(4)); // 6
console.log(iterator.next(5)); // 8
console.log(iterator.next());
````

**Throwing Errors in Iterators**

You can pass an error object to throw() that should be thrown when the iterator continues processing.

`````javascript
function *createIterator() {
    let first = yield 1;
    let second = yield first + 2;   // yield 4 + 2, then throw
    yield second + 3;   // never is executed
}

let iterator = createIterator();

console.log(iterator.next());
console.log(iterator.next(4));
console.log(iterator.throw(new Error("Boom")));
`````
You can catch such errors inside the generator using a try-catch block:

````javascript
function *createIterator() {
    let first = yield 1;
    let second;

    try {
        second = yield first + 2;   // yield 4 + 2, then throw
    } catch (ex) {
        second = 6; // on error, assign a different value
    }
    yield second = 3;
}

let iterator = createIterator();

console.log(iterator.next());
console.log(iterator.next(4));
console.log(iterator.throw(new Error('Boom')));
console.log(iterator.next());
````
**Delegating Generators**

Generators can delegate to other generators using a special from of yield with an asterisk (*), as long as the asterisk falls between the yield keyword and the generator function name.

`````javascript
function *createNumberIterator() {
    yield 1;
    yield 2;
}

function *createColorIterator() {
    yield 'red';
    yield 'green';
}


function *createCombinedIterator() {
    yield *createNumberIterator();
    yield *createColorIterator();
    yield true;
}

const iterator = createCombinedIterator();

console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());

{ value: 1, done: false }
{ value: 2, done: false }
{ value: 'red', done: false }
{ value: 'green', done: false }
{ value: true, done: false }
{ value: undefined, done: true }
`````

**Asynchronous Task Running**

````javascript
let fs = require("fs");

fs.readFile("./config.json", (err, contents) => {
    if (err) throw err;
    
    console.log(JSON.parse(contents));
    console.log("Done");
});
````

**A Simple Task Runner**

Because `yield` stops execution and waits for the `next()` method to be called before starting again, you can implement asynchronous calls without managing callbacks. To start, you need a function taht can call a generator and start the iterator, such as this:

````javascript
function run(taskDef) {
    // create the iterator, make available elsewhere
    let task = taskDef();

    // start the task
    let result = task.next();

    //recursive function to keep calling next()
    function step() {
        // if there's more to do
        if (!result.done) {
            result = task.next();
            step();
        }
    }

    // start the process
    step();
}

run(function*() {
    console.log(1);
    yield;
    console.log(2);
    yield;
    console.log(3);
});
````

The `run()` function accepts a task definition (a generator function) as an argument. It calls the generator to create an iterator and the result is stored for later use. The `step()` function checks wether `result.done` is false and if so, calls `next()` before recursivaly calling itselft. Each call to next() stores the return value in result, which is always overwritten to contain the latest information. The initial call to `step()` starts the process of looking at the result.done variable to see whether there's more to do.

**Task Running with Data**

The easiest way to pass data through the task runner is to pass the value specified by yield in to the next call to the `next()` meethod.

````javascript
function run(taskDef) {

    // create the iterator, make available elseewhere
    let task = taskDef();

    // start the task
    let result = task.next();

    // recursive function to keep calling next()
    function step() {

        // if there's more to do
        if(!result.done) {
            result = task.next(result.value);
            step();
        }
    }

    // start the process
    step();
}

run(function*() {
    let value = yield 1;
    console.log(value); // 1

    value = yield value + 3;
    console.log(value); // 4
});
````

**An Asynchronous Task Runner**

Because `yield` stops execution and waits for the `next()` method to be called before starting again, you can implement asynchronous calls without managing callbacks.

````javascript
function fetchData() {
    return function(callback) {
        callback(null, 'Hi!');
    };
}
````

This version of `fetchData()` introduces a 50ms delay before calling the callback, demonstrating that this pattern works equally well for synchronous and asynchronous code. You just have to make sure each function that wants to ve called using `yield` follows the same pattern.

````javascript
// Anytime result.value is a function, the task runner will execute it instead of just passing that value to the next() method.
const fs = require('fs');

function readFile(filename) {
    return function(callback) {
        fs.readFile(filename, callback);
    }
}

function fetchData() {
    return function(callback) {
        callback(null, 'Hi!');
    };
}

function run(taskDef) {
    
    // create the iterator, make available elsewhere
    let task = taskDef();

    // start the task
    let result = task.next();

    // resursive function to keep calling next()
    function step() {
        // if there's more to do
        if (!result.done) {
            if (typeof result.value === 'function') {
                result.value( (err, data) => {
                    if (err) {
                        result = task.throw(err);
                        return;
                    }

                    result = task.next(data);
                    step();
                });

            } else {
                result= task.next(result.value);
                step();
            }
        }
    }

    // start the process
    step();
}

run(function*() {
    let contents = yield readFile('config.json');
    console.log(JSON.parse(contents));
    console.log('Done');
});
````

## Introducting JavaScript Classes

**Class Declarations**

A basic class declaration begin with the `class` keyword followed by name of the class. 

````javascript
class PersonClass {

    constructor(name) {
        this.name = name;
    }

    sayName() {
        console.log(this.name);
    }
}

let person = new PersonClass('Nicholas');
person.sayName();

console.log(person instanceof PersonClass);
console.log(person instanceof Object);

console.log(typeof PersonClass);
console.log(typeof PersonClass.prototype.sayName);
````

* Class declarations, unlike function declarations, are not hoisted. Class declarations act like `let` declarations, so they exist in the temporal dead zone until execution reaches the declaration.
* All code inside class declarations runs in strict mode automatically. There's no way to opt out of strict mode inside classes.
* All methods are none numerable. This is a significant change from custom types, where you need to use `Object.defineProperty()` to make a method nonenumerable.
* All methods lack an internal `[[Construct]]` method and will throw an error if you try to call them with new.
* Calling the class constructor without new throws an error.
* Attempting to overwrite the class name within a class method throws an error.

With all of these differences in mind, the `PersonClass` declaration in the previous  example is directly equivalent to the following code, which doesn't use the class syntax:

````javascript
let PersonType2 = (function() {
    'use strict';

    const PersonType2 = function(name) {

        // make sure the function was called with new
        if (typeof new.target === 'undefined') {
            throw new Error('Constructor must be called with new.');
        }

        this.name = name;
    }

    Object.defineProperty(PersonType2.prototype, 'sayName', {
        value: function() {
            // make sure the method wasn't called with new
            if (typeof new.target !== 'undefined') {
                throw new Error('Method cannot be called with new');
            }

            console.log(this.name);
        },
        enumerable: false,
        writable: true,
        configurable: true
    });

    return PersonType2;

}());
````

**Class Expressions**

Classes and functions are similar in that they have two from: declarations  and expressions. Function and class declarations begin with an appropriate have an expression from that doesn't require an identifier after `function`, similarly, classes have and expression from that doesn't requiere an identifier after `class` . These *class expressions* are designed to be used in variable declarations or passed into functions as arguments.

**A Basic Class Expression**

````javascript
let PersonClass = class {
    constructor(name) {
        this.name = name;
    }

    sayName() {
        console.log(this.name);
    }
};

let person = new PersonClass('Nocholas');
person.sayName();

console.log(person instanceof PersonClass);
console.log(person instanceof Object);

console.log(typeof PersonClass);
console.log(typeof PersonClass.prototype.sayName);
````

As this example demonstrates, class expressions do not require identifiers after class. Aside from the syntax, class expressions are functionally equivalent to class declarations.

Whether you use class declarations or class expressions is mostly a matter of style.

**Accessor Properties**

You should create own properties inside class constructors, classes allow you to define accessor properties on the prototype. To create a getter, use the keyword get followed by a space, followed by an identifier; to create a setter, do the same using the keyword set, as shown here:

````javascript
class CustomHTMLElement {
    constructor(element) {
        this.element = element;
    }

    get html() {
        return this.element.innerHTML;
    }

    set html(value) {
        this.element.innerHTML = value;
    }
}

var descriptor = Object.getOwnPropertyDescriptor(CustomHTMLElement.prototype, "html");
console.log('get' in descriptor); // true
console.log('set' in descriptor); // true
console.log(descriptor.enumerable); // false
````

**Computed Member Names**

Class methods and accessor properties can also have computed names. Instead of using an identifier, use square brackets around an expression, which is the same syntax you use for object literal computed names. For example.

````javascript
let methodName = "sayName";

class PersonClass {
    constructor(name) {
        this.name = name;
    }

    [methodName]() {
        console.log(this.name);
    }
};

let me = new PersonClass('Nicholas');
me.sayName
````

**Generator Methods**

You can define a generator on an object literal by prepending an asterisk (*) to the method name. The same syntax works for classes as well, allowing any method to be a generator. Here's an example:

```javascript
class MyClass {
    *createIterator() {
        yield 1;
        yield 2;
        yield 3;
    }
}

let instance = new MyClass();
let iterator = instance.createIterator();
```

Generator methods are useful when you have an object that represents a collection of values an you want to iterate over those values easily. Defining a default iterator for your class is much more helpful if the class  represents a collection of values. You can define the default iterator for a class by using `Symbot.iterator` to define a generator method.

````javascript
class Collection {
    constructor() {
        this.items = [];
    }

    *[Symbol.iterator]() {
        yield *this.items;
    }
}

const collection = new Collection();
collection.items.push(1);
collection.items.push(2);
collection.items.push(3);

for (let x of collection) {
    console.log(x);
}
````

**Static Members**

ECMAScript 6 classes simplify the creation of static members by using the formal `static` annotation before the method or accessor property name.

```javascript
class PersonClass {
    // equivalent of the PersonType constructor
    constructor(name) {
        this.name = name;
    }

    // equivalent of PersonType.prototype.sayName
    sayName() {
        console.log(this.name);
    }

    // equivalent of PersonType.create
    static create(name) {
        return new PersonClass(name)
    }
}

let person = PersonClass.create('Nicholas');
```

You can use `static` keyword on any method or accessor property definition within a class. The only restriction is that you can't use static with the `constructor` method definition.

> Static members are not accessible from instances. You must always access static members from the class directly.

**Ingeritance with Derived Classes**

Classes make inheritance easier to implement by using the familiar `extends` keyword to specify the function from which the class should inherit. The prototypes are automatically adjusted, and you can access the base class constructor by calling the `super()` method.

```javascript
class Rectangle {

    constructor(length, width) {
        this.length = length;
        this.width = width;
    }

    getArea() {
        return this.length * this.width;
    }
}

class Square extends Rectangle {

    constructor(length) {
        // equivalent of Rectangle.call(this, length, length)
        super(length, length);
    }
}

const square = new Square(3);

console.log(square.getArea());
console.log(square instanceof Square);
console.log(square instanceof Rectangle);
```

> You can only use `super()` in a derived class constructor. If you try to use it in a non derived class (a class that doesn't use extends) or a function, it will throw an error.

> You must call `super()` before accessing `this` in the constructor. Because `super()` is responsible for initializing this, attempting to access `this` before calling `super()` results in an error.

> The only way to avoid calling super() is to return an object from the class constructor.

**Shadowing Class Methods**

````javascript
class Square extends Rectangle {

    constructor(length) {
        // equivalent of Rectangle.call(this, length, length)
        super(length, length);
    }

    getArea() {
        return this.length * this.length;
    }
}
````

Because the `getArea()` method is defined as part of Square, the method `Rectangle.prototype.getAre()` will no longer be called by any instances of Square. Of course, you can always decide to call the base class version of the method by using the `super.getArea()` method, like this:

````javascript
class Square extends Rectangle {

    constructor(length) {
        // equivalent of Rectangle.call(this, length, length)
        super(length, length);
    }

    getArea() {
        return super.getArea();
    }
}
````

**Inherited Static Members**

If a base class has static members, those static members are also available on the derived class. Inheritance works like that in other languages, buy this is a new concept in JavaScript.

````javascript
class Rectangle {

    constructor(length, width) {
        this.length = length;
        this.width = width;
    }

    getArea() {
        return this.length * this.width;
    }

    static create(length, width) {
        return new Rectangle(length, width);
    }
}

class Square extends Rectangle {

    constructor(length) {
        // equivalent of Rectangle.call(this, length, length)
        super(length, length);
    }
}

const rect = Square.create(3, 4);

console.log(rect instanceof Rectangle); // true
console.log(rect.getArea()); // 12
console.log(rect instanceof Square); // false
````

Through inheritance, the `static create()` method is available as `Square.create()` and behaves like the `Rectangle.create()`.

## Improved Array Capabilities

**The Array.of() Method**

The `Array.of()` method always creates an array containing its arguments regardless of the number of arguments or the arguments types. Here are some examples that use the `array.of()` method:

To create an array with the `Array.of()` method, just pass it the values you want in your array.

````javascript
let items = Array.of(1, 2);
console.log(items.length);
console.log(items[0]);
console.log(items[1]);
````

````javascript
let items = Array.of(2);
console.log(items.length);
console.log(items[0]);
````

````javascript
let items = Array.of("2");
console.log(items.length);
console.log(items[0]);
````

If you ever need to pass the Array constructor into a function, you might want to pass Array.of() instead to ensure constant behavior. For example:

````javascript
function createArray(arrayCreator, value) {
    return arrayCreator(value);
}
````

> The Array.of() method does not use the Symbol.species property to determine the type of return value. Instead, it uses the current constructor (this inside the of() method) to determine the correct data type to return.

**The Array.from() Method**

The `Array.from()` call creates a new array based on the items in arguments. So args is an in an instance of Array that contains the same values in the same positions as arguments.

> The `Array.from()` method also uses this to determine the type of array to return.

**New Methods on All Arrays**

ECMAScript 6  introduce the `find()` and `findIndex()` methods. Both `find()` `findIndex()` accept two arguments: a callback function and an optional value to use for `this` inside the callback function. The callback function is passed an array element, the index of that element in the array, the index of that element in the array and the actual array----the same arguments passed to methods like `map()` and `forEach()`. The callback should return `true` if the given value matches some criteria you define. Both `find()` and `findIndex()` also stop searching the array the first time the callback function returns the value, whereas `findIndex()` returns the index at which the value was found.

````javascript
let numbers = [25, 30, 35, 40, 45];

console.log(numbers.find(n => n > 33));
console.log(numbers.findIndex(n => n > 33));
````

This code calls `find()` and findIndex() to locate the first value in the numbers array that is greater than 33. The call to `find()` returns 35 and `findIndex()` returns 2, the location of 35 in the numbers array.

Both `find()` method `findIndex()` are usuful to find an array element that matches a condition rather than a value. If you only want to find a value `indexOf()` and lastIndexOf() are better choices.

**The fill() Method**

the `fill()` method fills one or more array elements with a specific value. When passed a value,`fill()` overwrites all the values in an array with that value.

````javascript
let numbers = [1, 2, 3, 4];
numbers.fill(1);

console.log(numbers.toString());
````

Here, the call to `numbers.fill(1)` changes all elements in numbers to 1. If you want to change only some of the elements rather than all of them, you can optionally include a start index and an exclusive end index.

```javascript
let numbers = [1, 2, 3, 4];
numbers.fill(1, 2);

console.log(numbers.toString());
numbers.fill(0, 1, 3);
console.log(numbers.toString());
```

**The copyWithin() Method**

The `copyWithin()` method is similar to `fill()` in that it changes multiple array elements at the same time, `copyWithin()` lets you copy array element values from the array. To accomplish that, you need to pass two arguments to the `copyWithin()` method: the index where the method should start filling values and the index where the values to be copied begin.

For instance, to copy the values from the fist two elements in an array in an array to the last two items in the array, you can do the following.

````javascript
let numbers = [1, 2, 3, 4];

// paste values into array starting at index 2
// copy values from array starting at index 0
numbers.copyWithin(2, 0);

console.log(numbers.toString()); // 1, 2, 1, 2
````

This code pastes values into numbers beginning from index 2, so indexes 2 an 3 will be overwritten. Passing 0 as the second argument to `copyWithin()` starts copying values from index 0 and continues until there are no more elements to copy into. By default, `copyWithin()` always copies values up to the end of the array by you can provide an optional thirdj argument to limit how many elements will be overwritten. That third argument is an exclusive an index at which copying of values stops. Here's how that looks in code:

````javascript
let numbers = [1, 2, 3, 4];

// paste values into array starting at index 2
// copy values from array starting at index 0
// stop copying values when you hit index 1
numbers.copyWithin(2, 0, 1);

console.log(numbers.toString()); // 1, 2, 1, 2
````

# Promises and Asynchornous Programming

JavaScript engines are built on the concept of a single-threaded event loop. *Single-threaded* means that only one pice of code is executed at a time. The *event loop* is a process inside the JavaScript engine that monitors code execution an manges the job queue. Keep in mind that as a queue, job execution rons from the first job in the queue to the last.

**Promise Basics**

A promise is a placeholder for the result of an asynchronous operation. Instead of subscribing to an event or passing a callback to a function, the function can return a promise, as shown here:

````javascript
// readFile promises to complete at some point in the future
let promise = readFile('example.txt');
````

**The Promise Life Cycle**

Each promise goes through a short life cycle starting in the ***pending*** state, which indicates that the asynchronous  operation hasn't complete yet.

A pending promise is considered ***unsettled***. Once the asynchronous operation completes, the promise is considered ***settled*** and enters one of two possible states:

* **Fulfilled** The promise's asynchronous operation has completed successfully.
* **Rejected** The promise's asynchronous operation didn't complete successfully due to either an error or some other cause.

An internal [[PromiseState]] property is set to "pending", ''fulfilled', or "rejected" to reflect the promise's state. This property isn't exposed on promise objects. But you can take a specific action when a promise changes state by using the `then()` method.

The `then()` method is present on all promises an takes two arguments. The first argument is a function to call when the promise is fulfilled. The second argument is a function to call when the promise is rejected. Similar to the fulfillment function.

> Any object that implements the `then()` method as described in the preceding paragraph is called a thenable. Al promises are thenables, bu all thenables are not promises.

Both arguments to `then()` are optional, so you can listen for any combination of fulfillment and rejection. For example, consider this set of `them()` calls:

````javascript
let fs = require("fs");

let promise = fs.readFile('config.json');

promise.then((contents) => {
    // fulfillment 
    console.log(contents);
}, (err) => {
    // rejection
    console.log(err.message);
});

promise.then((contents) => {
    // fulfillment
    console.log(contents);
});

promise.then(null, (err) => {
    // rejection
    console.error(err.message);
});
````

Promises also have a `catch()` method that behaves the same as `then()` when only a rejection handler is passed.  For example, the following `catch()` and `then()` calls are functionally equivalent:

````javascript
promise.catch((err) => {
    // rejection
    console.error(err.message);
});

// is the same as:

promise.then(null, (err) => {
    // rejection
    console.log(err.message);
});
````

The `then()` and `catch()` methods are intended to be used in combination to properly handle the result of asynchronous  operations. Just know that if you don't attach a rejection handler to a promise,  all failures will happen silenly. Always attach a rejection handler, even if the handler just logs the failure.

**Creating Unsettled Promises**

New promises are created using the `Promise` constructor. This constructor accepts a single argument: a function called the `executor`, which contains the code to initialize the promise. The executor is passed two functions named `resolve()` and `reject()` as arguments. The `resolve()` function is called when the executor has finished successfully to signal that the promise is ready to resolve, whereas the `reject()` function indicates that the executor has failed.

````javascript
// Node.js example

let fs = require('fs');

function readFile(filename) {
    return new Promise((resolve, reject) => {
        // trigger the asynchonous operation
        fs.readFile(filename, { encoding: 'utf8' }, (err, contents) => {
            // check for errors
            if (err) {
                reject(err);
                return;
            }

            // the read succeded
            resolve(contents);
        });
    });
}

let promise = readFile("config.json");
promise.then((contents) => console.log(contents), (err) => console.error(err.message));
````

Keep in mind that the executor runs immediately  when `readFile()` is called. When either `resolve()` or `reject()` is called inside the executor, a jov is added to the job queue to resolve the promise. This is called *job scheduling*. In job scheduling, you add a new job to the job queue to say "Don't execute this right now, by execute it later". For  instance, the `setTimeout()` function() function lets you specify a delay before a job is added to the queue:

Calling `resolve()` triggers an asynchronous operation. Functions passed to `then()` and `catch()` are executed asynchronously, because these are also added to the job queue .

```javascript
let promise = new Promise((resolve, reject) => {
    console.log('Promise');
    resolve();
});

promise.then( () =>  {
    console.log("Resolved");
});

console.log("Hi!");

// Promise
// Hi!
// Resolved
```

The reason is that fulfillment and rejection handlers are always added to the end of the job queue after the executor has completed.

**Chaining Promises**

A number of ways are are available to chain promises  together to accomplish more complex asynchronous behavior.

Each call to `them()` or `catch()` actually creates and returns another promise. This second promise is resolved only when the first has been fulfilled or rejected.

````javascript
let p1 = new Promise((resolve, reject) => resolve(42));

p1.then(
    (value) => console.log(value)
).then(
    () => console.log('Finished')
);

// 42
// Finished
````

The call to `p1.then()` returns a second prmise on which `then()` is called. The second `then()` fulfillment handler is only called after the first promise has been resolved. If you unchain this example, it looks like this:

````javascript
let p1 = new Promise((resolve, reject) => resolve(42));
let p2 = p1.then((value) => console.log(value));

p2.then(() => console.log('Finished'));
````

**Catching Errors**

Promise chaining allows you to catch errors that may occur in a fulfillment or rejection handler from a previous promise.

````javascript
let p1 = new Promise( (resolve, reject) => resolve(42) );

p1.then( value => { throw new Error('Boom!'); } )
    .catch( error => console.log(error.message)); // 'Boom!'
````

> *Always have a rejection handler at the end of a promise chain to ensure that you can properly handle any error that may occur.*

**Returning Values in Promise Chains**

Another important aspect of promise chains is the ability to pass data from one promise to the next.

````javascript
let p1 = new Promise( (resolve, reject) => resolve(42) );

p1.then(value => {
    console.log(value); // "42"
    return value + 1;
}).then( 
    value => console.log(value) // "43"
);
````

You could do the same thing with the rejection handler. When a rejection handler is called it may return a value. If it does, that value is used to fulfill the next promise in the chain, as in the next example.

```javascript
let p1 = new Promise( (resolve, reject) => reject(42) );

p1.catch( value => {
    // first fulfillment handler
    console.log(value); // "42"
    return value + 1;
}).then( value => {
    // second fulfillment handler
    console.log(value); // '43'
});
```

The failure of one promise can allow the recovery of the entire chain if necessary.

**Returning Promises in Promise Chains**

````javascript
let p1 = new Promise( (resolve, reject) => resolve(42) );
let p2 = new Promise( (resolve, reject) => resolve(43) );

p1.then( value => {
    // fist fulfillment handler
    console.log(value);
    return p2; // 42 from promise p2
}).then( value => {
    // second fulfillment handler
    console.log(value); // 43
});
````

The important thing to recognize about this pattern is that the second fulfillment handler is not aded to `p2` but rather to a third promise. The second fulfillment handler is therefore attached to that third promise, making the previous example  equivalent to this:

````javascript
let p1 = new Promise( (resolve, reject) => resolve(42) );

let p2 = new Promise( (resolve, reject) => resolve(43) );

let p3 = p1.then( value => {
    // fist fulfillment handler
    console.log(value); // 42
    return p2;
});

p3.then( value => {
    // second fulfillment handler
    console.log(value); // 43
});
````

You could attach a rejection handler instead:

````javascript
let p1 = new Promise( (resolve, reject) => resolve(42) );
let p2 = new Promise( (resolve, reject) => reject(43) );

p1.then( value => {
    // fist fulfillment handler
    console.log(value); // 42
    return p2;
}).catch( value => {
    // rejection handler
    console.log(value); // 43
});

````

Returning thenables from fulfillment or rejection handlers doesn't change when the promise executors are executed. Returning thenables simply allows you to define additional responses to the promise results.

````javascript
let p1 = new Promise( (resolve, reject) => resolve(42) );

p1.then( value => {
    console.log(value); // 42
    
    return new Promise( (resolve, reject) => resolve(43) );
}).then( value => console.log(value) );  // 43
````

This patter is useful when you want to wait until a previous promise has been settled before triggering another promise.

## Responding to Multiple Promises

Sometimes you'll want to monitor the progress of multiple promises to dermine the next action. ECMAScript 6 provides two methods that monitor multiple promises: `Promise.all()` and `Promise.race()`.

**Prmise.all()**

The `Promise.all()` method accepts a single argument, which is an iterable *(such an array)* of promises to monitor, an returns a promise that is resolved only when every promise in the iterable is resolved. The returned promise is fulfilled when every promise in the iterable is fulfilled.

````javascript
let p1 = new Promise( (resolve, reject) => resolve(42) );
let p2 = new Promise( (resolve, reject) => resolve(43) );
let p3 = new Promise( (resolve, reject) => resolve(44) );

p4 = Promise.all([p1, p2, p3]);

p4.then( value => {
    console.log(Array.isArray(value)); // true
    console.log( value[0] ); // 42
    console.log( value[1] ); // 43
    console.log( value[2] ); // 44
});
````

The call to `Promise.all()` creates promise `p4`, which is ultimately fulfilled when promises `p1, p2, and p3` are fulfilled.  The result passed to the fulfillment handler for `p4` is an array containing each resolved value: 42, 43, and 44. The values are stored in order in which the promises that resolved to them. 

​	If any promise passed to `Promise.all()` is rejected, the returned promise is immediately rejected without waiting for the other promises to complete:

````javascript
let p1 = new Promise( (resolve, reject) => resolve(42) );
let p2 = new Promise( (resolve, reject) => reject(43) );
let p3 = new Promise( (resolve, reject) => resolve(44) );

p4 = Promise.all([p1, p2, p3]);

p4.catch( value => {
    console.log(Array.isArray(value)); // true
    console.log(value);
});
````

The rejection handler for p4 is called immediately without waiting for p1 or p3 to finish executing. The rejection handler always receives a single value rather than an array, an the vaue is the rejection value from the promise that was rejected.

**The Promise.race() Method**

The `promise.race()` method provides a slightly different take on monitoring multiple promises. This method also accepts an iterable of promises to monitor and returns a promise, but the returned promise is settled as soon as the first promise is settled. 

```javascript
let p1 = Promise.resolve(42);
let p2 = new Promise( (resolve, reject) => resolve(43) );
let p3 = new Promise( (resolve, reject) => resolve(44) );
let p4 = Promise.race([p1, p2, p3]);

p4.then( value => console.log(value) ); // 42
```

The promises passed to `Promise.race()` are truly in a race to see which is settled first. If the first promise to settle is fulfilled, the returned promise is fulfilled and the the same case for a rejected promise.

```javascript
let p1 = new Promise( (resolve, reject) => resolve(42) );
let p2 = Promise.reject(43);
let p3 = new Promise( (resolve, reject) => resolve(44) );

let p4 = Promise.race([p1, p2, p3]);

p4.catch( value => console.log(value) ); // 43
```

Here, p4 is rejected because p2 is already in the rejected state when `Promise.rece()` is called. Even though p1 and p3 are fulfilled, those results are ignored because they occur after p2 is rejected.

**Inhering from Promises**

Just like other built-in types can use a promise as the base for a derived class. This allows you to define your own variation of promises to extend wha built-in promises can do. For instance, suppose you want to create a promise that can use methods named `success()` and `failure()` in addition to the usual `then()` and `catch()` methods. You could create that promise type as follows:

````javascript
class MyPromise extends Promise {
    
    // use default constructor

    success(resolve, reject) {
        return this.then(resolve, reject);
    }

    failure(reject) {
        return this.catch(reject);
    }
}

let promise = new MyPromise( (resolve, reject) => resolve(42) );

promise
    .success( value => console.log(value) )
    .failure( value => console.log(value) );
````

**Promise-Based Asynchronous Task Running**

Simplify the task runner by using promises as a common interface for al asynchronous code:

````javascript
let fs = require('fs');

function run(taskDef) {
    // create the iterator
    let task = taskDef();

    // start the task
    let result = task.next();

    // recursive function to iterate through
    (function step() {

        // if there's more to do
        if (!result.done) {

            // resolve to a promise to make it easy
            let promise = Promise.resolve(result.value);
            promise.then( value => {
                result = task.next(value);
                step();
            }).catch( error => {
                result = task.throw(error);
                step();
            });
        }
    }());
}


// define a function to use with the task runner

function readFile(filename) {
    return new Promise( (resolve, reject) => {
        fs.readFile( filename, function(err, contents) {
            if (err) {
                reject(err);
            } else {
                resolve(contents);
            }
        });
    });
}

// run task

run(function*() {
    let contents = yield readFile('config.json');
    console.log('The contents', JSON.parse(contents) );
    console.log('Done');
});
````

In this version of the code, a generic`run()` function executes a generator to create an iterator. It calls `task.next()` to astart the task and recursively calls `step()` until the iterator is complete.

`Promise.resolve()` just passes through any promise passed in and wraps any non-promise value and passes the value back to the iterator.

The only concern is ensuring that asynchronous functions like `readFile()` return a promise that correctly identifies its state. For Node.js built-in methods, that means you'll have to convert those methods to return promises instead of using callbacks.

**Future asynchronous task running**

The basic idea is to use a function marked with `async` instead of a generator an use await instead of yiled when calling a function, such as:

````javascript
(async function() {
    let contents = await readFile('config.json');
    doSomethingWith(contents);
    console.log('Done');
});
````

The `async` keyword before function indicates that the function is meant to run in an asynchronous manner. The `await` keyword signals that the function call to `readFile('config.json')` should return a promise, and if it doesn't, the response should be wrapped in a promise, `await` should return a promise, and if it doesn't the response is rejected and otherwise return the value from the promise.

The end result is that you can write asynchronous code as if it were synchronous without the overhead of managing iterator-based state machine.