# JavaScript 

## Fundamentals

### Variables

In JavaScript, we use  let keyword to declare variables. 



If you use a variable without initializing it, it will have an undefined value.

```javascript
let x; // x is the name of the variable

console.log(x); // undefined
```





### Constants

The const keyword was also introduced in the ES6(ES2015) version to create constants. 

Once a constant is initialized, we cannot change its value.

```javascript
const x = 5;
x = 10;  // Error! constant cannot be changed.
console.log(x)
```

Simply, a constant is a type of variable whose value cannot be changed.

Also, you cannot declare a constant without initializing it.

```javascript
const x;  // Error! Missing initializer in const declaration.
x = 5;
console.log(x)
```







### Data Types

There are eight basic data types in JavaScript. They are:

| Data Types  | Description                                        | Example                           |
| ----------- | -------------------------------------------------- | --------------------------------- |
| `String`    | represents textual data                            | `'hello'`, `"hello world!"` etc   |
| `Number`    | an integer or a floating-point number              | `3`, `3.234`, `3e-2` etc.         |
| `BigInt`    | an integer with arbitrary precision                | `900719925124740999n` , `1n` etc. |
| `Boolean`   | Any of two values: true or false                   | `true` and `false`                |
| `undefined` | a data type whose variable is not initialized      | `let a;`                          |
| `null`      | denotes a `null` value                             | `let a = null;`                   |
| `Symbol`    | data type whose instances are unique and immutable | `let value = Symbol('hello');`    |
| `Object`    | key-value pairs of collection of data              | `let student = { };`              |

Here, all data types except Object are primitive data types, whereas Object is non-primitive.



(1) String

```javascript
//strings example
const name = 'ram';
const name1 = "hari";
const result = `The names are ${name} and ${name1}`;
```

Single quotes and double quotes are practically the same and you can use either of them.

Backticks are generally used when you need to include variables or expressions into a string. This is done by wrapping variables or expressions with ${variable or expression} as shown above.



(2) Number

```javascript
const number1 = 3;
const number2 = 3.433;
const number3 = 3e5 // 3 * 10^5
```

A number type can also be +Infinity, -Infinity, and NaN (not a number). 

```javascript
const number1 = 3/0;
console.log(number1); // Infinity

const number2 = -3/0;
console.log(number2); // -Infinity

// strings can't be divided by numbers
const number3 = "abc"/3; 
console.log(number3);  // NaN
```



(3) BigInt

In JavaScript, Number type can only represent numbers less than (253 - 1) and more than -(253 - 1). However, if you need to use a larger number than that, you can use the BigInt data type.

A BigInt number is created by appending n to the end of an integer. 

```javascript
// BigInt value
const value1 = 900719925124740998n;

// Adding two big integers
const result1 = value1 + 1n;
console.log(result1); // "900719925124740999n"

const value2 = 900719925124740998n;

// Error! BitInt and number cannot be added
const result2 = value2 + 1; 
console.log(result2); 
```



(4) Boolean

```javascript
const dataChecked = true;
const valueCounted = false;
```



(5) undefined

The undefined data type represents value that is not assigned. If a variable is declared but the value is not assigned, then the value of that variable will be undefined.

```javascript
let name;
console.log(name); // undefined
```



(6) null

In JavaScript, null is a special value that represents empty or unknown value. 

```javascript
const number = null;
```



(7) Symbol

This data type was introduced in a newer version of JavaScript (from ES2015).

A value having the data type Symbol can be referred to as a symbol value. Symbol is an immutable primitive value that is unique. 

```javascript
// two symbols with the same description

const value1 = Symbol('hello');
const value2 = Symbol('hello');
```

Though value1 and value2 both contain 'hello', they are different as they are of the Symbol type.



(8) Object

An object is a complex data type that allows us to store collections of data.

```javascript
const student = {
    firstName: 'ram',
    lastName: null,
    class: 10
};
```





### Type Conversions

#### Implicit Conversion

(1) Implicit Conversion to String

When a number is added to a string, JavaScript converts the number to a string before concatenation.

```javascript
// numeric string used with + gives string type
let result;

result = '3' + 2; 
console.log(result) // "32"

result = '3' + true; 
console.log(result); // "3true"

result = '3' + undefined; 
console.log(result); // "3undefined"

result = '3' + null; 
console.log(result); // "3null"
```



(2) Implicit Conversion to Number

```javascript
// numeric string used with - , / , * results number type

let result;

result = '4' - '2'; 
console.log(result); // 2

result = '4' - 2;
console.log(result); // 2

result = '4' * 2;
console.log(result); // 8

result = '4' / 2;
console.log(result); // 2
```



(3) Non-numeric String Results to NaN

```javascript
// non-numeric string used with - , / , * results to NaN

let result;

result = 'hello' - 'world';
console.log(result); // NaN

result = '4' - 'hello';
console.log(result); // NaN
```



(4) Implicit Boolean Conversion to Number

```javascript
// if boolean is used, true is 1, false is 0

let result;

result = '4' - true;
console.log(result); // 3

result = 4 + true;
console.log(result); // 5

result = 4 + false;
console.log(result); // 4
```



(5) null Conversion to Number

```javascript
// null is 0 when used with number
let result;

result = 4 + null;
console.log(result);  // 4

result = 4 - null;
console.log(result);  // 4
```



(6) undefined used with number, boolean or null

```javascript
// Arithmetic operation of undefined with number, boolean or null gives NaN

let result;

result = 4 + undefined;
console.log(result);  // NaN

result = 4 - undefined;
console.log(result);  // NaN

result = true + undefined;
console.log(result);  // NaN

result = null + undefined;
console.log(result);  // NaN
```



#### Explicit Conversion

(1) Convert to Number Explicitly

To convert numeric strings and boolean values to numbers, you can use Number()

```javascript
let result;

// string to number
result = Number('324');
console.log(result); // 324

result = Number('324e-1')  
console.log(result); // 32.4

// boolean to number
result = Number(true);
console.log(result); // 1

result = Number(false);
console.log(result); // 0
```



(2) Convert to String Explicitly

To convert other data types to strings, you can use either String() or toString()

```javascript
//number to string
let result;
result = String(324);
console.log(result);  // "324"

result = String(2 + 4);
console.log(result); // "6"

//other data types to string
result = String(null);
console.log(result); // "null"

result = String(undefined);
console.log(result); // "undefined"

result = String(NaN);
console.log(result); // "NaN"

result = String(true);
console.log(result); // "true"

result = String(false);
console.log(result); // "false"

// using toString()
result = (324).toString();
console.log(result); // "324"

result = true.toString();
console.log(result); // "true"
```

Note: String() takes null and undefined and converts them to string. However, toString() gives error when null are passed.



(3) Convert to Boolean Explicitly

To convert other data types to a boolean, you can use Boolean()

```javascript
let result;
result = Boolean('');
console.log(result); // false

result = Boolean(0);
console.log(result); // false

result = Boolean(undefined);
console.log(result); // false

result = Boolean(null);
console.log(result); // false

result = Boolean(NaN);
console.log(result); // false
```





### Function

#### Declaring a Function

The syntax to declare a function is:

```javascript
function nameOfFunction () {
    // function body   
}
```



(1) Function Parameters

A function can also be declared with parameters.

And we do not need to declared parameter with data types.

<img src="https://raw.githubusercontent.com/a1254898873/images/master/202204051001051.png" alt="Working of JavaScript Function with parameter" style="zoom:50%;" />





(2) Function Return

The return statement can be used to return the value to a function call.

The return statement denotes that the function has ended. Any code after return is not executed.

If nothing is returned, the function returns an undefined value.

<img src="https://raw.githubusercontent.com/a1254898873/images/master/202204051001053.png" alt="Working of JavaScript Function with return statement" style="zoom:50%;" />



(3) Function Expressions

In Javascript, functions can also be defined as expressions. 

```javascript
// program to find the square of a number
// function is declared inside the variable
let x = function (num) { return num * num };
console.log(x(4));

// can be used as variable value for other variables
let y = x(3);
console.log(y);
```





#### Arrow Function

This function

```javascript
// function expression
let x = function(x, y) {
   return x * y;
}
```

can be written as

```javascript
// using arrow functions
let x = (x, y) => x * y;
```

If a function body has multiple statements, you need to put them inside curly brackets {}. 

```
let sum = (a, b) => {
    let result = a + b;
    return result;
}

let result1 = sum(5,7);
console.log(result1); // 12
```













#### Variable Scope

(1) Global Scope

In JavaScript, a variable can also be used without declaring it. If a variable is used without declaring it, that variable automatically becomes a global variable.

```javascript
function greet() {
    a = "hello"
}

greet();

console.log(a); // hello
```

In the above program, variable a is a global variable.

If the variable was declared using let a = "hello", the program would throw an error.



(2) Local Scope

```javascript
// program showing block-scoped concept
// global variable
let a = 'Hello';

function greet() {

    // local variable
    let b = 'World';

    console.log(a + ' ' + b);

    if (b == 'World') {

        // block-scoped variable
        let c = 'hello';

        console.log(a + ' ' + b + ' ' + c);
    }

    // variable c cannot be accessed here
    console.log(a + ' ' + b + ' ' + c);
}

greet();
```



#### Variable Hoisting

In terms of variables and constants, keyword var is hoisted and let and const does not allow hoisting.

```javascript
// program to display value
a = 5;
console.log(a);
var a; // 5
```

In the above example, variable a is used before declaring it. 

And the program works and displays the output 5

Generally, hoisting is not performed in other programming languages like Python, C, C++, Java.

Hoisting can cause undesirable outcomes in your program. And it is best to declare variables and functions first before using them and avoid hoisting.

In the case of variables, it is better to use let than var.





### Objects

#### Object Declaration

```javascript
const object_name = {
   key1: value1,
   key2: value2
}
```

You can also define an object in a single line.

```javascript
const person = { name: 'John', age: 20 };
```









#### Methods

In JavaScript, an object can also contain a function. 

```javascript
const person = {
    name: 'Sam',
    age: 30,
    // using function as a value
    greet: function() { console.log('hello') }
}

person.greet(); // hello
```

You can also add a method in an object. 

```javascript
// creating an object
let student = { };

// adding a property
student.name = 'John';

// adding a method
student.greet = function() {
    console.log('hello');
}

// accessing a method
student.greet(); // hello
```



#### Constructor

In JavaScript, a constructor function is used to create objects. 

Object Literal is generally used to create a single object. The constructor function is useful if you want to create multiple objects.

```javascript
// using constructor function
function Person () {
    this.name = 'Sam'
}

let person1 = new Person();
let person2 = new Person();
```

Note: It is considered a good practice to capitalize the first letter of your constructor function.

You can also create a constructor function with parameters. 

```javascript
// constructor function
function Person (person_name, person_age, person_gender) {

   // assigning  parameter values to the calling object
    this.name = person_name,
    this.age = person_age,
    this.gender = person_gender,

    this.greet = function () {
        return ('Hi' + ' ' + this.name);
    }
}


// creating objects
const person1 = new Person('John', 23, 'male');
const person2 = new Person('Sam', 25, 'female');

// accessing properties
console.log(person1.name); // "John"
console.log(person2.name); // "Sam"

```

You can also add properties and methods to a constructor function using a prototype.

```javascript
// constructor function
function Person () {
    this.name = 'John',
    this.age = 23
}

// creating objects
let person1 = new Person();
let person2 = new Person();

// adding new property to constructor function
Person.prototype.gender = 'Male';

console.log(person1.gender); // Male
console.log(person2.gender); // Male
```





#### Getter and Setter

In JavaScript, there are two kinds of object properties:

- Data properties
- Accessor properties

In JavaScript, accessor properties are methods that get or set the value of an object. For that, we use these two keywords:

- `get` - to define a getter method to get the property value
- `set` - to define a setter method to set the property value



Getter:

```javascript
const student = {

    // data property
    firstName: 'Monica',
    
    // accessor property(getter)
    get getName() {
        return this.firstName;
    }
};

// accessing data property
console.log(student.firstName); // Monica

// accessing getter methods
console.log(student.getName); // Monica

// trying to access as a method
console.log(student.getName()); // error
```



Setter:

```javascript
const student = {
    firstName: 'Monica',
    
    //accessor property(setter)
    set changeName(newName) {
        this.firstName = newName;
    }
};

console.log(student.firstName); // Monica

// change(set) object property using a setter
student.changeName = 'Sarah';

console.log(student.firstName); // Sarah
```

In JavaScript, you can also use Object.defineProperty() method to add getters and setters. 

```javascript
const student = {
    firstName: 'Monica'
}

// getting property
Object.defineProperty(student, "getName", {
    get : function () {
        return this.firstName;
    }
});

// setting property
Object.defineProperty(student, "changeName", {
    set : function (value) {
        this.firstName = value;
    }
});

console.log(student.firstName); // Monica

// changing the property value
student.changeName = 'Sarah';

console.log(student.firstName); // Sarah
```



#### Prototype

Prototype is used to provide additional property to all the objects created from a constructor function.

```javascript
// constructor function
function Person () {
    this.name = 'John',
    this.age = 23
}

// creating objects
const person1 = new Person();
const person2 = new Person();

// adding property to constructor function
Person.prototype.gender = 'male';

// prototype value of Person
console.log(Person.prototype);

// inheriting the property from prototype
console.log(person1.gender);
console.log(person2.gender);
```





#### Classes

(1) Creating JavaScript Class

```javascript
// creating a class
class Person {
  constructor(name) {
    this.name = name;
  }
}

// creating an object
const person1 = new Person('John');
const person2 = new Person('Jack');

console.log(person1.name); // John
console.log(person2.name); // Jack
```



(2) Methods

It is easy to define methods in the JavaScript class. You simply give the name of the method followed by `()`

```javascript
class Person {
    constructor(name) {
    this.name = name;
  }

    // defining method
    greet() {
        console.log(`Hello ${this.name}`);
    }
}

let person1 = new Person('John');

// accessing property
console.log(person1.name); // John

// accessing method
person1.greet(); // Hello John
```



(3) Getters and Setters

```javascript
class Person {
    constructor(name) {
        this.name = name;
    }

    // getter
    get personName() {
        return this.name;
    }

    // setter
    set personName(x) {
        this.name = x;
    }
}

let person1 = new Person('Jack');
console.log(person1.name); // Jack

// changing the value of name property
person1.personName = 'Sarah';
console.log(person1.name); // Sarah
```



(4) Inheritance

Inheritance enables you to define a class that takes all the functionality from a parent class and allows you to add more.

```javascript
// parent class
class Person { 
    constructor(name) {
        this.name = name;
    }

    greet() {
        console.log(`Hello ${this.name}`);
    }
}

// inheriting parent class
class Student extends Person {

}

let student1 = new Student('Jack');
student1.greet();
```









### Arrays

#### Create an Array

(1) Using an array literal

```javascript
const array1 = ["eat", "sleep"];
```



(2) Using the new keyword

```javascript
const array2 = new Array("eat", "sleep");
```

Note: It is recommended to use array literal to create an array.





#### Add an Element to an Array

The push() method adds an element at the end of the array. 

```javascript
let dailyActivities = ['eat', 'sleep'];

// add an element at the end
dailyActivities.push('exercise');

console.log(dailyActivities); //  ['eat', 'sleep', 'exercise']
```

The unshift() method adds an element at the beginning of the array.

```javascript
let dailyActivities = ['eat', 'sleep'];

//add an element at the start
dailyActivities.unshift('work'); 

console.log(dailyActivities); // ['work', 'eat', 'sleep']
```



#### Change the Elements of an Array

You can also add elements or change the elements by accessing the index value.

```javascript
let dailyActivities = [ 'eat', 'sleep'];

// this will add the new element 'exercise' at the 2 index
dailyActivities[2] = 'exercise';

console.log(dailyActivities); // ['eat', 'sleep', 'exercise']
```

Suppose, an array has two elements. If you try to add an element at index 3 (fourth element), the third element will be undefined. 

```javascript
let dailyActivities = [ 'eat', 'sleep'];

// this will add the new element 'exercise' at the 3 index
dailyActivities[3] = 'exercise';

console.log(dailyActivities); // ["eat", "sleep", undefined, "exercise"]
```



#### Remove an Element from an Array

You can use the pop() method to remove the last element from an array. The pop() method also returns the returned value. 

```javascript
let dailyActivities = ['work', 'eat', 'sleep', 'exercise'];

// remove the last element
dailyActivities.pop();
console.log(dailyActivities); // ['work', 'eat', 'sleep']

// remove the last element from ['work', 'eat', 'sleep']
const removedElement = dailyActivities.pop();

//get removed element
console.log(removedElement); // 'sleep'
console.log(dailyActivities);  // ['work', 'eat']
```

If you need to remove the first element, you can use the shift() method. The shift() method removes the first element and also returns the removed element. 

```javascript
let dailyActivities = ['work', 'eat', 'sleep'];

// remove the first element
dailyActivities.shift();

console.log(dailyActivities); // ['eat', 'sleep']
```



#### Array length

You can find the length of an element (the number of elements in an array) using the length property. 

```javascript
const dailyActivities = [ 'eat', 'sleep'];

// this gives the total number of elements in an array
console.log(dailyActivities.length); // 2
```



#### Array sort

```javascript
// custom sorting an array of strings
var names = ["Adam", "Jeffrey", "Fabiano", "Danil", "Ben"];

function len_compare(a, b){
    return a.length - b.length;
}

// sort according to string length
names.sort(len_compare);

console.log(names);
```

- If **returned value < 0**, a is sorted before b (a comes before b).
- If **returned value > 0**, b is sorted before a (b comes before a).
- If **returned value == 0**, a and b remain unchanged relative to each other.





#### for loop

```javascript
for (key in object) {
    // body of for...in
}
```

 Once you get keys, you can easily find their corresponding values.

```javascript
const student = {
    name: 'Monica',
    class: 7,
    age: 12
}

// using for...in
for ( let key in student ) {

    // display the properties
    console.log(`${key} => ${student[key]}`);
}
```



The for...of loop was introduced in the later versions of JavaScript ES6.

The for..of loop in JavaScript allows you to iterate over iterable objects (arrays, sets, maps, strings etc).

```javascript
for (element of iterable) {
    // body of for...of
}
```

```javascript
// array
const students = ['John', 'Sara', 'Jack'];

// using for...of
for ( let element of students ) {

    // display the values
    console.log(element);
}
```



And we can use forEach() to iterates over the elements of an array.

```javascript
array.forEach(function(currentValue, index, arr))
```

```javascript
let students = ['John', 'Sara', 'Jack'];

// using forEach
students.forEach(myFunction);

function myFunction(item, index, arr) {

    // adding strings to the array elements
    arr[index] = 'Hello ' + item;
}

console.log(students);
```





### Map

Map is similar to objects in JavaScript that allows us to store elements in a key/value pair.

The elements in a Map are inserted in an insertion order. However, unlike an object, a map can contain objects, functions and other data types as key.



#### Create JavaScript Map

```javascript
// create a Map
const map1 = new Map(); // an empty map
console.log(map1); // Map {}
```



#### Insert Item to Map

After you create a map, you can use the set() method to insert elements to it. 

```javascript
// create a set
let map1 = new Map();

// insert key-value pair
map1.set('info', {name: 'Jack', age: 26});
console.log(map1); // Map {"info" => {name: "Jack", age: 26}}
```

You can also use objects or functions as keys. 

```javascript
// Map with object key
let map2 = new Map();

let obj = {};
map2.set(obj, {name: 'Jack', age: "26"});

console.log(map2); // Map {{} => {name: "Jack", age: "26"}}
```



#### Access Map Elements

You can access Map elements using the get() method. 

```javascript
let map1 = new Map();
map1.set('info', {name: 'Jack', age: "26"});

// access the elements of a Map
console.log(map1.get('info')); // {name: "Jack", age: "26"}
```





#### Removing Elements

```javascript
let map1 = new Map();
map1.set('info', {name: 'Jack', age: "26"});

// removing a particular element
map1.delete('address'); // false
console.log(map1); // Map {"info" => {name: "Jack", age: "26"}} 

map1.delete('info'); // true
console.log(map1); // Map {}
```

```javascript
let map1 = new Map();
map1.set('info', {name: 'Jack', age: "26"});

// removing all element
map1.clear();
console.log(map1); // Map {}
```



#### Map Size

```javascript
let map1 = new Map();
map1.set('info', {name: 'Jack', age: "26"});

console.log(map1.size); // 1
```





### Set

#### Create JavaScript Set

```javascript
// create Set
const set1 = new Set(); // an empty set
console.log(set1); // Set {}

// Set with multiple types of value
const set2 = new Set([1, 'hello', {count : true}]);
console.log(set2); // Set {1, "hello", {count: true}}
```



#### Adding New Elements

```javascript
const set = new Set([1, 2]);
console.log(set.values());

// adding new elements
set.add(3);
console.log(set.values());

// adding duplicate elements
// does not add to Set
set.add(1);
console.log(set.values());
```



#### Removing Elements

```javascript
const set = new Set([1, 2, 3]);
console.log(set.values()); // Set Iterator [1, 2, 3]

// removing a particular element
set.delete(2);
console.log(set.values()); // Set Iterator [1, 3]
```

```javascript
const set = new Set([1, 2, 3]);
console.log(set.values()); // Set Iterator [1, 2, 3]

// remove all elements of Set
set.clear();
console.log(set.values()); // Set Iterator []
```











### Exceptions 

try catch Statement：

```javascript
try {
    // try_statements
} 
catch(error) {
    // catch_statements  
}
finally() {
    // codes that gets executed anyway
}
```

```javascript
const numerator= 100, denominator = 'a';

try {
     console.log(numerator/denominator);
     console.log(a);
}
catch(error) {
    console.log('An error caught'); 
    console.log('Error message: ' + error);  
}
finally {
     console.log('Finally will execute every time');
}
```



throw Statement：

```javascript
const number = 40;
try {
    if(number > 50) {
        console.log('Success');
    }
    else {

        // user-defined throw statement
        throw new Error('The number is low');
    }

    // if throw executes, the below code does not execute
    console.log('hello');
}
catch(error) {
    console.log('An error caught'); 
    console.log('Error message: ' + error);  
}
```





### Modules

You need to use the export keyword for the particular function, objects, etc. to import them and use them in other files.

```javascript
// exporting the variable
export const name = 'JavaScript Program';

// exporting the function
export function sum(x, y) {
    return x + y;
}
```

```javascript
import { name, sum } from './module.js';

console.log(name);
let add = sum(4, 9);
console.log(add); // 13
```

```javascript
// in greet.js
function greet() {
    // strict by default
}

export greet();
```





### Proxies

(1) For Validation

You can use a proxy for validation. You can check the value of a key and perform an action based on that value.

```javascript
let student = {
    name: 'Jack',
    age: 24
}

const handler = {

    // get the object key and value
    get(obj, prop) {

    // check condition
    if (prop == 'name') {
      return obj[prop];
    } else {
      return 'Not allowed';
    }
  }
}

const proxy = new Proxy(student, handler);
console.log(proxy.name); // Jack
console.log(proxy.age); // Not allowed
```



(2) Read Only View of an Object

There may be times when you do not want to let others make changes in an object. In such cases, you can use a proxy to make an object readable only. 

```javascript
let student = {
    name: 'Jack',
    age: 23
}

const handler = {
    set: function (obj, prop, value) {
        if (obj[prop]) {
            
            // cannot change the student value
            console.log('Read only')
        }
    }
};

const proxy = new Proxy(student, handler);

proxy.name = 'John'; // Read only
proxy.age = 33; // Read only
```



(3) Side Effects

You can use a proxy to call another function when a condition is met. 

```javascript
const myFunction = () => {
    console.log("execute this function")
};

const handler = {
    set: function (target, prop, value) {
        if (prop === 'name' && value === 'Jack') {
            // calling another function
            myFunction();
        }
        else {
            console.log('Can only access name property');
        }
    }
};

const proxy = new Proxy({}, handler);

proxy.name = 'Jack'; // execute this function
proxy.age = 33; // Can only access name property
```





### Asynchronous

#### setTimeout

```javascript
setTimeout(function, milliseconds);
```

```javascript
// program to display a text using setTimeout method
function greet() {
    console.log('Hello world');
}

setTimeout(greet, 3000);
console.log('This message is shown first');
```

```
This message is shown first
Hello world
```

 If you want to stop this function call, you can use the clearTimeout() method.

```javascript
// program to stop the setTimeout() method

let count = 0;

// function creation
function increaseCount(){

    // increasing the count by 1
    count += 1;
    console.log(count)
}

let id = setTimeout(increaseCount, 3000);

// clearTimeout
clearTimeout(id); 
console.log('setTimeout is stopped.');
```

```
setTimeout is stopped.
```





#### setInterval

```javascript
// program to display a text using setInterval method
function greet() {
    console.log('Hello world');
}

setInterval(greet, 1000);
```

```
Hello world
Hello world
Hello world
Hello world
Hello world
....
```





#### Promise

(1) Create a Promise

```javascript
let promise = new Promise(function(resolve, reject){
     //do something
});
```

The Promise() constructor takes a function as an argument. The function also accepts two functions resolve() and reject().

If the promise returns successfully, the resolve() function is called. And, if an error occurs, the reject() function is called.

```javascript
const count = true;

let countValue = new Promise(function (resolve, reject) {
    if (count) {
        resolve("There is a count value.");
    } else {
        reject("There is no count value");
    }
});

console.log(countValue);
```

```
Promise {<resolved>: "There is a count value."}
```



(2) Promise Chaining

<img src="https://raw.githubusercontent.com/a1254898873/images/master/202204052304496.png" alt="Working of JavaScript promise" style="zoom:50%;" />

The then() method is used with the callback when the promise is successfully fulfilled or resolved.

```javascript

// returns a promise

let countValue = new Promise(function (resolve, reject) {
  resolve("Promise resolved");
});

// executes when promise is resolved successfully

countValue
  .then(function successValue(result) {
    console.log(result);
  })

  .then(function successValue1() {
    console.log("You can call multiple functions this way.");
  });
```

The catch() method is used with the callback when the promise is rejected or if an error occurs. 

```javascript
// returns a promise
let countValue = new Promise(function (resolve, reject) {
   reject('Promise rejected'); 
});

// executes when promise is resolved successfully
countValue.then(
    function successValue(result) {
        console.log(result);
    },
 )

// executes if there is an error
.catch(
    function errorValue(result) {
        console.log(result);
    }
);
```

You can also use the finally() method with promises. The finally() method gets executed when the promise is either resolved successfully or rejected. 

```javascript
// returns a promise
let countValue = new Promise(function (resolve, reject) {
    // could be resolved or rejected   
    resolve('Promise resolved'); 
});

// add other blocks of code
countValue.finally(
    function greet() {
        console.log('This code is executed.');
    }
);
```

<img src="https://raw.githubusercontent.com/a1254898873/images/master/202204052303734.png" alt="Working of JavaScript promise chaining" style="zoom: 50%;" />

```javascript

api().then(function(result) {
    return api2() ;
}).then(function(result2) {
    return api3();
}).then(function(result3) {
    // do work
}).catch(function(error) {
    //handle any error that may occur before this point 
});
```

```javascript
const request = url => { 
    return new Promise((resolve, reject) => {
        $.get(url, data => {
            resolve(data)
        });
    })
};

// 请求data1
request(url).then(data1 => {
    return request(data1.url);   
}).then(data2 => {
    return request(data2.url);
}).then(data3 => {
    console.log(data3);
}).catch(err => throw new Error(err));
```





#### async

We use the async keyword with a function to represent that the function is an asynchronous function. The async function returns a promise.

```javascript
async function f() {
    console.log('Async function.');
    return Promise.resolve(1);
}

f().then(function(result) {
    console.log(result)
});
```





#### await

The await keyword is used inside the async function to wait for the asynchronous operation.

```javascript
// a promise
let promise = new Promise(function (resolve, reject) {
    setTimeout(function () {
    resolve('Promise resolved')}, 4000); 
});

// async function
async function asyncFunc() {

    // wait until the promise resolves 
    let result = await promise; 

    console.log(result);
    console.log('hello');
}

// calling the async function
asyncFunc();
```





### JSON

```javascript
// JSON syntax
{
    "name": "John",
    "age": 22,
    "gender": "male",

}
```

Note: JSON data requires double quotes for the key.

JSON array is written inside square brackets [ ]. 

```javascript
// JSON array
[ "apple", "mango", "banana"]

// JSON array containing objects
[
    { "name": "John", "age": 22 },
    { "name": "Peter", "age": 20 }.
    { "name": "Mark", "age": 23 }
]
```

Converting JSON to JavaScript Object:

```javascript
// json object
const jsonData = '{ "name": "John", "age": 22 }';

// converting to JavaScript object
const obj = JSON.parse(jsonData);

// accessing the data
console.log(obj.name); // John
```

Converting JavaScript Object to JSON:

```javascript
// JavaScript object
const jsonData = { "name": "John", "age": 22 };

// converting to JSON
const obj = JSON.stringify(jsonData);

// accessing the data
console.log(obj); // "{"name":"John","age":22}"
```





### Closures

In JavaScript, closure provides access to the outer scope of a function from inside the inner function, even after the outer function has closed. 

```javascript
// javascript closure example

// outer function
function greet() {

    // variable defined outside the inner function
    let name = 'John';

    // inner function
    function displayName() {

        // accessing name variable
        return 'Hi' + ' ' + name;
      
    }

    return displayName;
}

const g1 = greet();
console.log(g1); // returns the function definition
console.log(g1()); // returns the value
```

```
function displayName() {
      // accessing name variable
      return 'Hi' + ' ' + name;
  }
Hi John
```











# TypeScript

## Debug

```
{
        "name": "Launch index.ts",
        "type": "node",
        "request": "launch",
        "runtimeArgs": [
            "-r",
            "ts-node/register"
        ],
        "args": [
            "${workspaceFolder}/src/index.ts"
        ]
}
```

we should add runtimeArgs into the file called launch.json







## Basic Types

### Number Type

```typescript
let price: number;
```

Or you can initialize the price variable to a number:





### String Type

```typescript
let firstName: string = 'John';
let title: string = "Web Developer";
```

```typescript
let firstName: string = `John`;
let title: string = `Web Developer`;
let profile: string = `I'm ${firstName}. 
I'm a ${title}`;

console.log(profile);
```





### Object Type

```typescript
let employee: object;

employee = {
    firstName: 'John',
    lastName: 'Doe',
    age: 25,
    jobTitle: 'Web Developer'
};

console.log(employee);
```

To explicitly specify properties of the employee object, you first use the following syntax to declare the employee object:

```typescript
let employee: {
    firstName: string;
    lastName: string;
    age: number;
    jobTitle: string;
};
```

And then you assign the employee object to a literal object with the described properties:

```typescript
employee = {
    firstName: 'John',
    lastName: 'Doe',
    age: 25,
    jobTitle: 'Web Developer'
};
```







### Array Type

```typescript
let arrayName: type[];
```

```typescript
let skills: string[];
```

And you can add one or more strings to the array:

```typescript
skills[0] = "Problem Solving";
skills[1] = "Programming";
```

or use the push() method:

```typescript
skills.push('Software Design');
```

The following illustrates how to declare an array that hold both strings and numbers:

```typescript
let scores = ['Programming', 5, 'Software Design', 4]; 
```

```typescript
let scores : (string | number)[];
scores = ['Programming', 5, 'Software Design', 4]; 
```





### Tuple type

A tupple is an array with a fixed number of elements whose types are known.

```typescript
let skill: [string, number];
skill = ['Programming', 5];
```







### Enum Types

A TypeScript enum is a group of constant values.

```typescript
enum name {constant1, constant2, ...};
```

```typescript
enum ApprovalStatus {
    draft,
    submitted,
    approved,
    rejected
};
```

```typescript
const request =  {
    id: 1,
    status: ApprovalStatus.approved,
    description: 'Please approve this request'
};

if(request.status === ApprovalStatus.approved) {
    // send an email
    console.log('Send email to the Applicant...');
}
```







### Any Type

The TypeScript any type allows you to store a value of any type. It instructs the compiler to skip type checking.

Use the any type to store a value that you don’t actually know its type at the compile-time or when you migrate a JavaScript project over to a TypeScript project.

```typescript
let result: any;
```





### Never Type

The never type contains no value.

The never type represents the return type of a function that always throws an error or a function that contains an indefinite loop.

```typescript
function raiseError(message: string): never {
    throw new Error(message);
}
```

The return type of the following function is inferred to the never type:

```typescript
function reject() { 
   return raiseError('Rejected');
}
```





### Union Types

A TypeScript union type allows you to store a value of one or serveral types in a variable.

```typescript
let result: number | string;
result = 10; // OK
result = 'Hi'; // also OK
result = false; // a boolean value, not OK
```





### Type Aliases

```typescript
type alias = existingType;
```

```typescript
type alphanumeric = string | number;
let input: alphanumeric;
input = 100; // valid
input = 'Hi'; // valid
input = false; // Compiler error
```





### String Literal Types

A TypeScript string literal type defines a type that accepts specified string literal.

```typescript
type MouseEvent: 'click' | 'dblclick' | 'mouseup' | 'mousedown';
let mouseEvent: MouseEvent;
mouseEvent = 'click'; // valid
mouseEvent = 'dblclick'; // valid
mouseEvent = 'mouseup'; // valid
mouseEvent = 'mousedown'; // valid
mouseEvent = 'mouseover'; // compiler error

let anotherEvent: MouseEvent;
```









## Functions

### declare a function

```typescript
function name(parameter: type, parameter:type,...): returnType {
   // do something
}
```

If a function does not return a value, you can use the void type as the return type. The void keyword indicates that the function doesn’t return any value.

```typescript
function echo(message: string): void {
    console.log(message.toUpperCase());
}
```

When you do not annotate the return type, TypeScript will try to infer an appropriate type. 

```typescript
function add(a: number, b: number) {
    return a + b;
}
```





### Optional Parameters

To make a function parameter optional, you use the ? after the parameter name. 

```typescript
function multiply(a: number, b: number, c?: number): number {

    if (typeof c !== 'undefined') {
        return a * b * c;
    }
    return a * b;
}
```

- First, use the `?` after the `c` parameter.
- Second, check if the argument is passed to the function by using the expression `typeof c !== 'undefined'`.





### Default Parameters

```typescript
function name(parameter1:type=defaultvalue1, parameter2:type=defaultvalue2,...) {
   //
}
```

```typescript
function applyDiscount(price: number, discount: number = 0.05): number {
    return price * (1 - discount);
}

console.log(applyDiscount(100)); // 95
```





### Rest Parameters

A rest parameter allows you a function to accept zero or more arguments of the specified type.

```typescript
function fn(...rest: type[]) {
   //...
}
```

```typescript
function getTotal(...numbers: number[]): number {
    let total = 0;
    numbers.forEach((num) => total += num);
    return total;
}
```





## Classes

### declare a class

```typescript
class Person {
    ssn: string;
    firstName: string;
    lastName: string;

    constructor(ssn: string, firstName: string, lastName: string) {
        this.ssn = ssn;
        this.firstName = firstName;
        this.lastName = lastName;
    }

    getFullName(): string {
        return `${this.firstName} ${this.lastName}`;
    }
}
```

```typescript
let person = new Person(171280926, 'John', 'Doe');
```





### Access Modifiers

The private modifier

```typescript
class Person {
    private ssn: string;
    private firstName: string;
    private lastName: string;
    // ...
}
```

Once the private property is in place, you can access the ssn property in the constructor or methods of the Person class. 

```typescript
class Person {
    private ssn: string;
    private firstName: string;
    private lastName: string;

    constructor(ssn: string, firstName: string, lastName: string) {
        this.ssn = ssn;
        this.firstName = firstName;
        this.lastName = lastName;
    }

    getFullName(): string {
        return `${this.firstName} ${this.lastName}`; 
    }
}
```



The public modifier

If you don’t specify any access modifier for properties and methods, they will take the public modifier by default.

```typescript
class Person {
    // ...
    public getFullName(): string {
        return `${this.firstName} ${this.lastName}`; 
    }
    // ...
}
```





The protected modifier

The protected modifier allows properties and methods of a class to be accessible within same class and within subclasses.

```typescript
class Person {

    protected ssn: string;
    
    // other code
}

```







### Static Methods and Properties

Static properties and methods are shared by all instances of a class.

```typescript
class Employee {
    private static headcount: number = 0;

    constructor(
        private firstName: string,
        private lastName: string,
        private jobTitle: string) {

        Employee.headcount++;
    }

    public static getHeadcount() {
        return Employee.headcount;
    }
}
```





### Abstract Classes

```typescript
abstract class Employee {
    //...
}

```

```typescript
abstract class Employee {
    constructor(private firstName: string, private lastName: string) {
    }
    abstract getSalary(): number
    get fullName(): string {
        return `${this.firstName} ${this.lastName}`;
    }
    compensationStatement(): string {
        return `${this.fullName} makes ${this.getSalary()} a month.`;
    }
}
```

The getSalary() method is an abstract method. The derived class will implement the logic based on the type of employee.





### Inheritances

```typescript
class Employee extends Person {
    //..
}
```

To call the constructor of the parent class in the constructor of the child class, you use the super()

```typescript
class Employee extends Person {
    constructor(
        firstName: string,
        lastName: string,
        private jobTitle: string) {
        
        // call the constructor of the Person class:
        super(firstName, lastName);
    }
}
```







## Interfaces

### type checking

TypeScript interfaces define contracts in your code and provide explicit names for type checking.

```typescript
interface Person {
    firstName: string;
    lastName: string;
}
```

```typescript
function getFullName(person: Person) {
    return `${person.firstName} ${person.lastName}`;
}

let john = {
    firstName: 'John',
    lastName: 'Doe'
};

console.log(getFullName(john));
```

The getFullName() function will accept any argument that has two string properties. 

 And it doesn’t have to have exactly two string properties. 

```typescript
let jane = {
   firstName: 'Jane',
   middleName: 'K.'
   lastName: 'Doe',
   age: 22
};
```

Since the jane object has two string properties firstName and lastName, you can pass it into the getFullName() function as follows:

```typescript
let fullName = getFullName(jane);
console.log(fullName); // Jane Doe
```









### Function types

To describe a function type, you assign the interface to the function signature that contains the parameter list with types and returned types.

```typescript
interface StringFormat {
    (str: string, isUpper: boolean): string
}
```

```typescript
let format: StringFormat;

format = function (str: string, isUpper: boolean) {
    return isUpper ? str.toLocaleUpperCase() : str.toLocaleLowerCase();
};

console.log(format('hi', true));
```









### Class Types

Interfaces are typically used as class types that make a contract between unrelated classes.

```typescript
interface Json {
   toJSON(): string
}
```

```typescript
class Person implements Json {
    constructor(private firstName: string,
        private lastName: string) {
    }
    toJson(): string {
        return JSON.stringify(this);
    }
}

```









## Generics

### introduction of generics

The T allows you to capture the type that is provided at the time of calling the function. 

```typescript
function getRandomElement<T>(items: T[]): T {
    let randomIndex = Math.floor(Math.random() * items.length);
    return items[randomIndex];
}
```

```typescript
let numbers = [1, 5, 7, 4, 2, 9];
let randomEle = getRandomElement<number>(numbers); 
console.log(randomEle);
```





### Generic Interfaces

Generic interfaces that describe object properties

```typescript
interface Pair<K, V> {
    key: K;
    value: V;
}

```

```typescript
let month: Pair<string, number> = {
    key: 'Jan',
    value: 1
};

console.log(month);
```



Generic interfaces that describe methods

```typescript
interface Collection<T> {
    add(o: T): void;
    remove(o: T): void;
}

```

```typescript
class List<T> implements Collection<T>{
    private items: T[] = [];

    add(o: T): void {
        this.items.push(o);
    }
    remove(o: T): void {
        let index = this.items.indexOf(o);
        if (index > -1) {
            this.items.splice(index, 1);
        }
    }
}

```



Generic interfaces that describe index types

```typescript
interface Options<T> {
    [name: string]: T
}

let inputOptions: Options<boolean> = {
    'disabled': false,
    'visible': true
};

```





### Generic Classes

```typescript
class Stack<T> {
    private elements: T[] = [];

    constructor(private size: number) {
    }
    isEmpty(): boolean {
        return this.elements.length === 0;
    }
    isFull(): boolean {
        return this.elements.length === this.size;
    }
    push(element: T): void {
        if (this.elements.length === this.size) {
            throw new Error('The stack is overflow!');
        }
        this.elements.push(element);

    }
    pop(): T {
        if (this.elements.length == 0) {
            throw new Error('The stack is empty!');
        }
        return this.elements.pop();
    }
}

```

```typescript
let numbers = new Stack<number>(5);
```









