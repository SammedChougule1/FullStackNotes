## primitive datatype
- string
- number
- boolean
- null
- undefined
- bigint
- symbol

## Array Loops
### Mutating array loops
- push()
- pop()
- shift()
- reverse()
- sort()
- splice()

### Not Mutating array
- slice
- map : applies given function and return new array
- filter : return elements that pass the test implemented by the provided function
- some : if any element statisfies the condition
- every : every element satisfies the condition
- find : if element is present else undefined.
- findIndex : finds index else -1
- reduce : Applies a function against an accumulator and each element in the array (from left to right) to reduce it to a single value.
```Javascript 
const sum = array.reduce((accumulator, currentValue) => {
  return accumulator + currentValue;
}, 0);

```

## first class function 
functions in that language are treated like any other variable.function can be passed as an argument
## first order function
function that doesn’t accept another function as an argument and doesn’t return a function as its return value.
## higher order function
function that accepts another function as an argument or returns a function as a return value or both
## unary function
function that accepts exactly one argument
## currying function
Currying is the process of taking a function with multiple arguments and turning it into a sequence of functions each with only a single argument
```Javascript
const multiArgFunction = (a, b, c) => a + b + c;
console.log(multiArgFunction(1, 2, 3)); // 6

const curryUnaryFunction = (a) => (b) => (c) => a + b + c;
curryUnaryFunction(1); // returns a function: b => c =>  1 + b + c
curryUnaryFunction(1)(2); // returns a function: c => 3 + c
curryUnaryFunction(1)(2)(3); // returns the number 6
```
## pure function
always returns same value in parameters passed are same

##  Let VS Var
- let 
    - let statement declares a block scope local variable
    - Hoisted but not initialized
- var
    - var keyword used to define a variable globally, is has  function scope.
    - Variable declaration will be hoisted, initialized as undefined

## Temporal Dead Zone
is a specific period or area of a block where a variable is inaccessible until it has been intialized with a value.IIFE is to obtain data privacy because any variables declared within the IIFE cannot be accessed by the outside world

## IIFE (Immediately Invoked Function Expression)
runs as soon as it is defined
```Javascript
(function () {
  // logic here
})();
```

###  decode or encode a URL
```Javascript
let uri = "employeeDetails?name=john&occupation=manager";
let encoded_uri = encodeURI(uri);
let decoded_uri = decodeURI(encoded_uri);
```

## closures
in an nested function the inner function has access to data and variables of outer function even if outer function is finished executing.The closure has three scope chains.
1. Own scope where variables defined between its curly brackets
2. Outer function's variables
3. Global variables

## Service worker 
A Service worker is basically a script (JavaScript file) that runs in the background, separate from a web page and provides features that don't need a web page or user interaction.Service worker can't access the DOM directly. But it can communicate with the pages it controls by responding to messages sent via the postMessage interface, and those pages can manipulate the DOM.

## IndexedDB
IndexedDB is a low-level API for client-side storage of larger amounts of structured data, including files/blobs. This API uses indexes to enable high-performance searches of this data.

## web storage
browsers can store key/value pairs locally within the user's browser.they are not cookies. two mechanisms for storing data on the client.
1. Local storage: It stores data for current origin with no expiration date.
2. Session storage: It stores data for one session and the data is lost when the browser tab is closed.
- LocalStorage is the same as SessionStorage but it persists the data even when the browser is closed and reopened(i.e it has no expiration time) whereas in sessionStorage data gets cleared when the page session ends.

```Javascript
// check ig web storage is present
if (typeof Storage !== "undefined") {
  // Code for localStorage/sessionStorage.
} else {
  // Sorry! No Web Storage support..
}

// Save data to sessionStorage
sessionStorage.setItem("key", "value");

// Get saved data from sessionStorage
let data = sessionStorage.getItem("key");

// Remove saved data from sessionStorage
sessionStorage.removeItem("key");

// Remove all saved data from sessionStorage
sessionStorage.clear();

// storage event handler // triggers on storage change
window.onstorage = functionRef;
```

## Cookie
 Cookies are saved as key/value pairs on local.You can delete a cookie by setting the expiry date as a passed date
 ```Javascript
document.cookie = "username=John";
document.cookie = "username=John; expires=Sat, 8 Jun 2019 12:00:00 UTC";
document.cookie = "username=John; path=/services";
// delete cookie
document.cookie ="username=; expires=Fri, 07 Jun 2019 00:00:00 UTC; path=/;";
 ```

 ## callback function 
 function passed into another function as an argument. This function is invoked inside the outer function to complete an action
 ```javascript
function callbackFunction(name) {
  console.log("Hello " + name);
}

function outerFunction(callback) {
  let name = prompt("Please enter your name.");
  callback(name);
}

outerFunction(callbackFunction);
 ```

## server-sent events
Server-sent events (SSE) is a server push technology enabling a browser to receive automatic updates from a server via HTTP connection without resorting to polling. These are a one way communications channel - events flow from server to client only. This has been used in Facebook/Twitter updates, stock price updates, news feeds etc.
| Event    | Description                                       |
|----------|---------------------------------------------------|
| onopen   | It is used when a connection to the server is opened |
| onmessage| This event is used when a message is received      |
| onerror  | It happens when an error occurs                    |

```Javascript
if (typeof EventSource !== "undefined") {
  var source = new EventSource("sse_generator.js");
  source.onmessage = function (event) {
    document.getElementById("output").innerHTML += event.data + "<br>";
  };
}
```

## promise.all
Promise.all is a promise that takes an array of promises as an input (an iterable), and it gets resolved when all the promises get resolved or any one of them gets rejected. For example, the syntax of promise.all method is below

```Javascript
Promise.all([Promise1, Promise2, Promise3]) .then(result) => {   console.log(result) }) .catch(error => console.log(`Error in promises ${error}`))
```

## event flow
order in which event is received on the web page.
1. Top to Bottom(Event Capturing)
2. Bottom to Top (Event Bubbling) Default

### event bubbling
event propagation where the event first triggers on the innermost target element, and then successively triggers on the ancestors (parents) of the target element in the same nesting hierarchy till it reaches the outermost DOM element.
By default, event handlers triggered in event bubbling phase
### event capturing
event propagation where the event is first captured by the outermost element, and then successively triggers on the descendants (children).

You need to pass true value for addEventListener method to trigger event handlers in event capturing phase.
```Javascript
<div>
  <button class="child">Hello</button>
</div>

<script>
  const parent = document.querySelector("div");
  const child = document.querySelector(".child");

  parent.addEventListener("click",
    function () {
      console.log("Parent");
    },
    true
  );

  child.addEventListener("click", function () {
    console.log("Child");
  });
</script>
// Parent
// Child
```