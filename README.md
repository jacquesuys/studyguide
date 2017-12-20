# CS + JS
## Intro
JS consists of **statements** and **expressions**:
A program is a sequence of statements. Here is an example of a **statement**, which declares (creates) a variable foo:

Statements “do things.” 
```
var foo;
```
Expressions produce values.
```
3 * 7
```
### By reference vs. value
**By value:** primitive types when assigned to variables and variables to those variables does so by value, meaning they get copied and take up more memory.
```
const a = 2;
a; //2 (0x01)
const b = a;
b; //2 (0x02)
```
**By reference:** when variables are assigned to objects they point to the same object.
This means that shared objects can be mutated by their respective variable.
```
var a = {greeting: 'hello'};
a; // {greeting: 'hello'} (0x01)

var b = a;
b.greeting = 'hola'; // mutate
a; // {greeting: 'hola'} (0x01)

// even, by reference as paramter
const change = (obj) => obj.greeting = 'hi';

change(a);
a; // {greeting: 'hi'} (0x01)
b; // {greeting: 'hi'} (0x01)

// equals operator sets up new memory space
b = { greeting: 'hallo' };
a; // {greeting: 'hi'} (0x01)
b; // {greeting: 'hallo'} (0x02)
```
### Functions
Functions are objects; we can add keys and methods to them (think OOP).
```
const greet = () => {
  console.log(greet.language);
}

greet.language = "Afrikaans";

greet() // "Afrikaans"
```

**Function declarations:**

Function declarations (function declarations are also hoisted)
```
function add(param1, param2) {
    return param1 + param2;
}
```

**Function expressions:**

```
const add = (param1, param2) => param1 + param2;
```
### Functional Programming (FP)
Teaches you ways to structure your code to make it maintainable, compose able, and easy to reason about. It also minimizes side effects and minimizes the impact on state.

Javascript has always been a FP language, but the concepts of FP has seen a resurgence because of JS being used to write web components/applications (where state is very important).

Functions can be assigned to variables one of the features of functional programming.
*(It can be created dynamically that is why we say: JS, has first class functions)*.

Another tennant of FP is higher-order functions where we pass a function as an argument into another function.
This is what is known as composibility where 2 functions compose together.
Take ```.map``` for instance; map is an iterative method (function) that takes the callback (the function being passed) as the predicate on which the iterative method operates.

The advantage of the above mentioned is that we only worry about the predicate (callback) and the type of array/value the iteration should produce (reduce, map, filter, every, reject)

### Functors
Functors are mappable objects.
```
console.log([ 2, 4, 6 ].map(x => x + 3))
// => [ 5, 7, 9 ]
```
That means arrays are functors. This includes, strings, objects and functions.

Categories have two important properties:

1. Identity
2. Composition

#### Identity

The contract for functors are they should map values and return the equivalent functor type (Array => Array).

#### Composition

“The second law says that composing two functions and then mapping the resulting function over a functor should be the same as first mapping one function over the functor and then mapping the other one.”

```
F.map(x => f(g(x))) // notice the nesting

// Equals

F.map(g).map(f)

```

### Immutable
An immutable object is one whose content cannot be changed:
1. To improve performance (no planning for the object's future changes)
2. To reduce memory use (make object references instead of cloning the whole object)
3. Thread-safety (multiple threads can reference the same object without interfering with one other)

### Closures
A closure is a function that contains a returned function, which has access to the parent's scope.
Upon the second invocation the returned function produces a result.
```
const inc = (a) => {
  return () => ++a;
}

const init = inc(3);
init(); // 4
init(); // 5
```
The other benefit is that is can also be subsequently called an produce "incremented" values.

### IIFE TODO
Immediatelty invoked function expressions are anonymous functions being 

### Event-Driven Programming
The flow of the program is determined by async events: user inputs or server events `GET` requests.

### Promises
The Promise object represents the eventual completion (or failure) of an asynchronous operation, and its resulting value.
```
let myFirstPromise = new Promise((resolve, reject) => {
  setTimeout(() => { // async
    resolve("Success!");
  }, 250);
});

myFirstPromise
  .then((msg) => console.log("Yay! " + msg) );
```

### Network
1. Browser contacts the DNS server to fnd the IP address of URL 
2. DNS returns back the IP address of the site 
3. Browser opens TCP connection to the web server at port 80
4. Browser fetches the html code of the page requested
5. Browser renders the HTML in the display window
6. Browser terminates the connection when window is closed

[What happens next: in-depth](https://github.com/alex/what-happens-when)

## Sorting Algorithms

**Big-O notation** is how we express the efficiency of an alogrithm.
We mainly look at the time it takes to complete an operation given a certain amount of inputs, but in certain cases we also look at the memory usage, which can also be expressed using the notation.

[Big-O cheat sheet](http://bigocheatsheet.com/)

### Bubble Sort
```
const bubbleSort = arr => {
  let swopped = true;
  while (swopped) {
    swopped = false;
    arr.forEach((num, i) => {
      if (num > arr[i + 1]) {
        const temp = num;
        arr[i] = arr[i + 1];
        arr[i + 1] = temp;
        swopped = true;
      }
    });
  }
}

// O(n^2) quadratic
```
### Insertion Sort
```
const insertionSort = arr => {
  let i, j, rmvd;
  for (i = 1; i < arr.length; i++) {
    for (j = 0; j < i; j++) {
      if (arr[i] < arr[j]) {
        rmvd = arr.splice(i, 1);
        arr.splice(j, 0, rmvd[0]);
      }
    }
  }
}

// O(n^2) quadratic
```
### Merge Sort
```
const splitter = nums => {
  if (nums.length < 2) return nums;

  const half = Math.floor(nums / 2);
  const left = nums.slice(0, half);
  const right = nums.slice(half, nums.length);

  return mergeSort(splitter(left), splitter(right));
}

const mergeSort = (left, right) => {
  const result = [];
  
  while (left.length && right.length) {
    if (left[0] <= right[0]) {
      result.push(left.shift());
    } else {
      result.push(right.shift());
    }
  }

  return result.concat(left, right);
}

// O(log(n)) logarithmic
```
### Quick Sort
```
const quickSort = nums => {
  if (nums.length <= 1) return nums;
  
  const pivot = nums[nums.length - 1];
  const left = [];
  const right = [];
  let dir = '';
  let i = 0
  
  for (; i < nums.length - 1; i++) {
    dir = nums[i] < pivot ? left : right;
    dir.push(nums[i]);
  }
  return [...quickSort(left), pivot, ...quickSort(right)];
};

// O(log(n)) logarithmic
```

## React
1. React is a library for building interfaces.
2. React is the "V" in MVC. 
3. Data flows **one-way** (unidirectionally).
4. It doesn't have **KVO** (Key Value Observable) penalties.
5. Uses a Virtual DOM and virtual events (synthetic events).

### What is ReactDOM?
React is used all over—web, mobile, desktop, VR, appliances.

Instead of it just being one enormous library, there are multiple platform libraries. **ReactDOM is the web companion library.**

### Components
Components are functions. They can be **Class-** or **Stateless** Component, more on that in a bit...
```
function Greeting() {
  return <h1>Hello 🎄</h1>;
}
```
And can be used as:
```
<Greeting />
// ! Remember to always use caps
// Otherwise JSX will think it is a DOM element
```

### Props
Props are function (component) arguments.
Passed down props are immutable.
```
// Parent
<Greeting name="Bulbasaur" />

// Child component
function Greeting(props) {
  return <h1>Hello {props.name}!</h1>;
}

// => Hello Bulbasaur!
```

### Class Components
Extending components with the `React.Component` class, components get access to following properties and methods:

1. A render method (in which you can use JSX)
2. Life-cycle methods (API)
3. State via the `this.state` key or `this.setState` method

aka "Smart Components, Parent Components".

### Stateless Components
Has no state as implied in the name, but it has a return value.
```
function Greeting(props) {
  return <h1>Hello {props.name}!</h1>;
}
```
aka "Dumb components, Child components"
## TODO:

FED masters ES6 advantages for components; fat arrow, no constructor

Diff kinds of state

FED masters, React Component State quiz, objects vs functions, object.assign, 2nd args callback

## State Architecture Patterns

1. Shared Component State

2. Container Pattern

3. Higher Order Component

4. Render Properties Pattern

this keyword
3 Principles of Redux
thunk
monads
currying
middleware

virtual dom shadow dom

dependency injection

Virtual vs Shadow DOM:
[http://www.slideshare.net/ScottSword/shadow-dom-virtual-dom](http://www.slideshare.net/ScottSword/shadow-dom-virtual-dom)
[http://programmers.stackexchange.com/questions/225400/pros-and-cons-of-facebooks-react-vs-web-components-polymer](http://programmers.stackexchange.com/questions/225400/pros-and-cons-of-facebooks-react-vs-web-components-polymer)

https://react.holiday/2017/1/

https://medium.com/@dtinth/what-is-a-functor-dcf510b098b6
https://medium.com/javascript-scene/functors-categories-61e031bac53f