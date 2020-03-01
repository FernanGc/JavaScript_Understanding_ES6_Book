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