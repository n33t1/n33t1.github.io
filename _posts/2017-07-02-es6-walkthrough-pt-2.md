---
layout: post
title: "ES6 Walkthrough Pt.2"
description: "Notes for the rally coding ES6 Javascript course"
date: 2017-07-04
tags: [dev, javascript]
comments: true
share: true
---

1. [Default Function Arguments](#a1)
2. [Rest and Spread Operator](#a2)
3. [Destructuring](#a3)
4. [Classes](#a4)
5. [Generators](#a5)
6. [Promises and Fetch](#a6)


### <a name="a1"></a>Specifying Default Function Arguments

We can assign default values for function arguments and make the arguments optional. For example, we can make a sum function to add two elements whose default values are both 0.
```javascript
function sum(a, b) {
  if (a === undefined) {
    a = 0; 
  }
  
  if (b === undefined) {
    b = 0; 
  }
  
  return a + b;
}
// is equal to

function sum(a = 0, b = 0) {
  return a + b;
}
```
Another example for default function arguments. Here the user can either passing in style or not. 

```javascript
function addOffset(style) {
  if (!style) {
    style = {}; 
  }
  
  style.offset = '10px';
  
  return style;
}

//is equvalent to
function addOffset(style = {}) {
  style.offset = '10px';
  
  return style;
}

```

### <a name="a2"></a>Rest and Spread Operator

* Rest Operator:    
When passing in multiple arguments, we can use rest operator to capture a list of arguments. For example, if we have an addNumbers function, with too many numbers as parameters to add, say like `addNumbers(1,2,3,4,5)`, in which case our corresponding function will be like `addNumbers(a,b,c,d,e)`. But with rest operator, we can simplify the above arguments to just `addNumbers(...numbers)`. 

```javascript
function addNumbers(a,b,c,d,e){
    const numbers = [a,b,c,d,e];
    return numbers.reduce((sum, number) => {
        return sum += number;
    }, 0);
}

addNumbers(1,2,3,4,5); //15

// is equivalent to 
function addNumbers(...numbers){
    return numbers.reduce((sum, number) => {
        return sum += number;
    }, 0);
}

addNumbers(1,2,3,4,5); //15
```

* **Spread Operator:**    
Spread Operator is similar to js concat function, but with more freedom (you can add a single element to the concated arrays). You use spread operator when you want to merge diffenent arrays or elements together. 

```javascript
const defaultColors = ['red', 'green'];
const userFavColors = ['orange', 'yellow'];

defaultColors.concat(userFavColors); //['red', 'green','orange', 'yellow']

// or we can use sread operators insteadly

const defaultColors = ['red', 'green'];
const userFavColors = ['orange', 'yellow'];
const fallColors = ['fire red', 'fall orange'];
[ 'green', 'blue', ...fallColors, ...defaultColors, ...userFavColors];//['red', 'green','orange', 'yellow']
//[ 'blue', 'fire red', 'fall orange', 'red', 'green','orange', 'yellow']

```
In a nutshell, we often use rest operator to capture multiple arguments into a list. We use spread operator to merge lists together.

``` javascript
function unshift(array, a, b, c, d, e) {
  return [a, b, c, d, e].concat(array);
}

//is equivalent to

function unshift(array, ...newArray) {
  return [...newArray,...array];
}

//or 
const unshift = (array, ...newArray) => [...newArray,...array];
```

### <a name="a3"></a>Destructuring

ES6's syntactic sugar makes destructuring easier. We can directly use `{}` to embrace all the variables we want to destruct from the json object. Say, we want to destruct type and amount from expense, we can just have an one line statement like `const {type, amount} = expense;`. 

``` javascript
var expense = {
    type : 'Business',
    amount : '$45 USD'
};

var type  = expense.type;
var amount = expense.amount;

//ES6

const {type, amount} = expense;
type;
amount;
```

When passing in arguments, this destructuring syntactic sugar can also be useful. We can destruct the variables we want directly when passing in the arguments, instead of inside of the function. 
``` javascript
var savedFiled = {
    extension: '.jpg',
    name: 'repost',
    size: 14040
}

function fileSummay(file) {
    return `The file ${file.name}. ${file.naextension} is of size ${file.size}.`
}

// is equivalent to

function fileSummay({ name, extension, size }, {color}) {
    return `The file ${name}. ${extension} is of size ${size}.`
}

fileSummay(savedFile, {color: 'red'}); // red The file repost.jpg is of size 14040
```
Also, when we are destructuring elements from array, we can use this es6 feature. If we try to pass in more than the actual number of elements existed in the array, the extra element will return as undefined. We can also use rest operator to ignore the elements we don't want at the end.

``` javascript
const companies = [
    'Google',
    'Facebook',
    'Uber'
];
const firstCompany = companies[0];
//is equvailent to
const [ name, name2, name3, name4 ] = companies;
name; //Google
name2; //Facebook
name3; //Uber
name4; //undefined

const companies = [
    'Google',
    'Facebook',
    'Uber'
];
const firstCompany = companies[0];
//is equvailent to
const [ name, name2, ...rest ] = companies;
rest; //["Uber"]
```

We can also mix destructuring arrays and objects together. When we have an array of json objects like `const table = [{location:1},{location:2}]`, we can use `const [location] = table` to destruct the first json object and `const [{location}] = table` to destruct the first value for key 'location' from the first json object in the array.

``` javascript
const companies = [
    { name: 'Google', location: 'Mountain View'}, 
    { name: 'Facebook', location: 'Menlo Park'},
    { name: 'Uber', location: 'San Francisco'}
];

// destructuring first object
const [location] = companies; 
location;//{ name: 'Google', location: 'Mountain View'}

// destructuring value for key 'location' from first object

const [{location}] = companies;
location; //Mountain View

// another example: 

const Google = {
    locations: ['Mountain View', 'New York', 'London']
};

// array
const { locations: locations } = Google;
locations; // ["Mountain View", "New York", "London"]

// or
const { locations } = Google;
locations; // ["Mountain View", "New York", "London"]

// first object in the array

const { locations: [location] } = Google;
locations; // Mountain View
```
Typically, we use destructuring in two cases: destructuring objects and destructuring array. Argument objects is a good usage of destructuring
. We can destruct them into objects when passing all the strings as parameters gets taxing.

```javascript
function signup(username, password, email, dateOfBirth, city) {
    //create new user
}
//other code
//other code
//other code
//other code

signup('myname', 'mypassword', 'myemail@example.com', '1/1/1990', 'New York');

// is equivalent to

const user = {
    username: 'myname',
    password: 'mypassword',
    email: 'myemail@example.com',
    dateOfBirth: '1/1/1990',
    city: 'New York'
}

signup(user);

//and signup becomes: 
function signup({username, password, email, dateOfBirth, city}) {
    //create new user
}
```

Another useage of destructuring array.

```javascript
const points = [
    [4, 5],
    [10, 1],
    [0, 40]
];

points.map(pair => {
    const [x, y] = pair;
});

// is equivalent to
points.map([x, y] => {
    return {x: x, y: y}
});

// is equivalent to
points.map([x, y] => {
    return {x, y}
});
```


Recursion with destructuring is a good practice mixing what we learned above. 
> Use array destructuring, recursion, and the rest/spread operators to create a function 'double' that will return a new array with all values inside of it multiplied by two without using any array helpers.    
> Input:
>  
>double([1,2,3])
>
>Output
>
> [2,4,6]


```javascript
const numbers = [1, 2, 3];

function double([ number, ...rest] = numbers) {
    if (!number) {
        return [];
    }
    return [number * 2, ...double(rest)];
}


const numbers = [1, 2, 3];

function double([ number, ...rest ]) {
    return rest.length ? [number*2, ...double(rest)] : [number * 2];
}
```



### <a name="a4"></a>Classes
With ES6, we don't need to use the traditional javascript prototype inheritence anymore. We are able to write a pretty standard class definitions in javascript now.

```javascript
class Car {
    constructor({ title }) {
        this.title = title;
    }

    drive() {
         return 'vroom';
    }
}

const car = new Car({ title: 'Toyota' });
car.drive();

function Toyota(options) {
    Car.call(this, options);
    this.color = options.color;
}

//class inheritance `super()` is used to call constructor from parent class. whenever call `super()`, remember to pass the parameters used in parent class as well. 
class Toyota extends Car {
    constructor(options) {
        super(options); // Car.constructor
        this.color = options.color;
    }

    honk() {
        return 'beep';
    }
}

const toyota = new Toyota({ color: 'red', title: 'Daily Driver' });
toyota.honk(); //beep
toyota; //{ 'color' : 'red', 'title' : 'Daily Driver' }
```

Classes are widely used in React when creating components.

```javascript
class MyComponent extends Component {
    doSomething() {

    }
    
    doSomethingElse() {

    }
}
```

When we want to use class inheritance, we call `Class child extends parent {}`. And inside of the constructor, we use `super()` to inherit the values from parent class. Subclassing Monsters is a good practice for class inheritance. 

> Now that you have a monster created, create a subclass of the Monster called Snake.  
>
> The Snake should have a 'bite' method.  The only argument to this method is another instance of a Snake.
>
> The instance of Snake that is passed in should have their health deducated by 10

```javascript
class Monster {
  constructor(options) {
    this.health = 100;
    this.name = options.name;
  }
}

class Snake extends Monster {
    constructor(options){
        super(options);
    }
    
    bite(snake){
        return snake.health -= 10;
    }
}
```

### <a name="a5"></a>Generators

For let loop is an iterating tool just like foreach or map. In the example below, we are iterating over an array using for let loop.
```javascript
const colors = ['red', 'green', 'blue'];

//for let loop
for (let color of colors) {
    console.log(color);
}
```

**Rule of thumb:** If there is an undefined error showing up for function, it is highly likely that you forgot to return the value.

* **what is a generator**

What is a generator? Imao I don't have a deep understading in it yet... I also don't know the difference between iterator and generator... But basically, when you call .next() function, it will update the state only to the next state, until it is done. It's like if you have a for loop, and you call break at the end of each loop, the generator function will remember where you are and store the current stage. Then you call and execute the loop again, the generator will retrieve the value you stored, and update from there.

```javascript
function *numbers() {
    yield
}

const gen = numbers();
gen.next(); // {'done': false}
gen.next(); // {'done': true}
gen.next(); // {'done': true}
```

```javascript
function *shopping() {
    const stuffFromStore = yield 'cash';
    const cleanClothes = yield 'laundry';
    return [stuffFromStore, cleanClothes];
}

const gen = shopping();
gen.next(); // {"value": "cash", "done": false}

gen.next('groceries'); // {"value": "laundry", "done": false}
gen.next('clean clothes'); // {"value": ["groceries", "clean clothes"], "done": false}
```

Given the features we discussed before, generator is useful for for let loop. 

```javascript
function* colors() {
    yield 'red';
    yield 'blue';
    yield 'green';
}
const gen = colors();
gen.next(); //red done false
gen.next(); //blue done false
gen.next(); //green done false
gen.next(); //done

const myColors = [];
for (let color of colors()) {
    myColors.push(color);
}
myColors; 
```
When we are processing data from json files, we can also use generators. Generator delegation allows you to combine multiple generators. You can skip fileds in json file you dont want. The steps to do so goes like:     
1. If you want to use generator delegation, namely merge mutiple json objects you want, select one object, and put the rest inside of that json object. In the example, we put `testingTeam` inside of `engineeringTeam`.
2. Then, start with `function *`, we write generators for each of the json objects we want. Then we are also gonna put the sub generators into a main generator, as how we call `testingTeamIterator` inside of `TeamIterator`.
3. We use for let loop to loop over the generator and push the values we want into an array or whatever you want.



```javascript
const testingTeam = {
    lead: 'Amanda',
    tester: 'Bill'
}

const engineeringTeam = {
    testingTeam,
    size: 3,
    department: 'Engineering',
    lead: 'Jill',
    meanager: 'Alex',
    engineering: 'Dave'
}

function* TeamIterator(team) {
    yield team.lead;
    yield team.manager;
    yield team.engineer;
    const testingTeamGenerator = testingTeamIterator(team.testingTeam);
    yield* testingteamGenerator;
}

function* TestingTeamIterator(team) {
    yield team.lead;
    yield team.tester;
}

const names = [];
for (let name of TeamIterator(engineeringTeam)){
    names.push(name);
}
name; // ["Jill", "Alex", "Dave", "Amanda", "Bill"]
```

But with **Symbol.iterator**, we can further clean up our code. In the previous algorithm, we need to write seperate generators for each json objects. But with **Symbol.iterator**, we are able to select the json fields directly inside of the object. And then inside of the for let loop, we just call the main json object. And then we are done.

```javascript

const testingTeam = {
    lead: 'Amanda',
    tester: 'Bill',
    [Symbol.iterator]: function* () {
        yield this.lead;
        yield this.tester;
    }
};

const engineeringTeam = {
    testingTeam,
    size: 3,
    department: 'Engineering',
    lead: 'Jill',
    meanager: 'Alex',
    engineering: 'Dave',
    [Symbol.iterator]: function* () {
        yield this.lead;
        yield this.manager;
        yield this.engineer
        // yield* means it will find the iterator and iterate
        yield* this.testingTeam;
    }
};

const names = [];
for (let name of engineeringTeam){
    names.push(name);
}
name; // ["Jill", "Alex", "Dave", "Amanda", "Bill"]
```

* **generator with recursions**

In web dev, we also use trees. Comments sections on reddit is an example of trees. But how do we implement tree data structure with javascript? We use generator with recursions! And how we do generator with recursions? Inside of the desired class, use `*[Symbol.iterator]` to call the generator. We 1) yield content for the parent comments, and 2) we use for let loop to yield the contents for the child comments.

```javascript
class Comment {
    constructor(content, children) {
        this.content = content;
        this.children = children;
    }

    *[Symbol.iterator] () {
        yield this.content;
        for (let child of this.children) {
            yield* child;
        }
    }
}

const children = [
    new Comment('good comment', [new Comment("thanks man", [new Comment("yea bro", [])])]),
    new Comment('nad comment', []),
    new Comment('meh', [])
];

const tree = new Comment('Great Post!', children);
tree; 
//{"content":"Great Post!","children":[{"content":"good comment","children":[{"content":"thanks man","children":[{"content":"yea bro","children":[]}]}]},{"content":"nad comment","children":[]},{"content":"meh","children":[]}]}

const values = [];
for (let value of tree) {
    values.push(value);
}

values; 
//["Great Post!","good comment","thanks man","yea bro","nad comment","meh"]
```

### <a name="a6"></a>Promises

ES6 has built-in Promise function. Basically, if everything worked out, have have status `resolve();` and the callback will be `then()`. If it failed, the status would be `reject()` and we go into the `catch()` for call back.

```javascript
promise = new Promise((resolve, reject) => {
    resolve();
});

promise
    .then(() => { console.log("finally finished!"); })
    .then(( => { console.log("dadhasdhsakjhas!"); }));
    .catch(() => { console.log("oh no!"); });
//finally finished!
//dadhasdhsakjhas!
promise;
```

There are 3 states of promises: 1) unresolved, which means waiting for something to finish, 2) resolved, which means something finished and it all went ok, and 3) rejected, which means something finished and went bad.     

Code below is an example for async code with promises. We can use `setTimeout()` or `onload()` depends on what we need.
```javascript
promise = new Promise((resolve, reject) => {
    var request = newXHTMLRequest()
    request.onload = () => {
        resolve();
    };
});
```

Fetch is bascially a built-in function for GET? we have to return `response.json()` in order to get the real data. But fetch will not return any server error above 3000. It will only goes into the catch block when the network request fails.

```javascript
url = "";
fetch(url)
    .then( response => response.json())
    .catch(error => console.log('BAD', error));
```

```javascript
function makeAjaxRequest(url, method) {
    if (!method) {
        method = 'GET';
    }
}
//equals to 

function makeAjaxRequest(url, method = 'GET') {
    return method;
}
```
```javascript
makeAjaxRequest('google.com'); // output GET
makeAjaxRequest('google.com', 'POST'); // POST
```

**Rule of thumb:**: undefined != null

If we use `makeAjaxRequest('google.com', null); // output GET`, we are actually passing value of `null` to the fuction. As this parameter is not empty, we wont get our default value as output. We eventually will get nothing. However, with ```makeAjaxRequest('google.com', undefined); // output GET```, we will have the GET request.
