# Higher-Order Functions

In this write up we will quickly go over what higher-order functions are in Javascript and why you would want to use them.

## What are Higher-Order Functions

Functions that operate on other functions, either by taking them as arguments or by returning them, are called higher-order functions.

Let's look at a few examples

1. Functions that create new Functions

```
function addBy(n) {
  return function(m) {return n + m; };
}
var addBy2 = addBy(2)
console.log(addBy2(2)) // 4
```

2. Functions that change other functions
```
function printNumbersBy(checker) {
  return function(num) {
    for (let i = 1; i <= num; i++) {
      if (checker(i)) {
        console.log(i);
      }
    }
  };
}

function isEven(value) {
  if (value % 2 === 0) {
    return true
  }
  return false
}

printNumbersBy(isEven)(10); // 2 4 6 8 10
```

3. Functions that provide new types of control flow
```
function unless(test, then) {
  if (!test) then();
}

function repeat(times, body) {
  for (var i = 0; i < times; i++) body(i);
}

repeat(3, function(n) {
  unless(n % 2, function() {
    console.log(n, "is even");
  });
});
// → 0 is even
// → 2 is even
```

## Abstraction

Let's take a quick break from the code and talk about a few very important concepts in programming. Abstraction is a way to hide implementation details so that we can focus on problems at a higher level.

For example even programming languages are an abstractions to the zeros and ones that the language creates for us.

Abstractions allows us to to reason about large programs and problems in a more simplistic way.

## Imperative Programing

With the concept of abstractions in mind let's demonstrate two styles of programming that shows abstractions in action.

imperative programing defines a style of programming where the program describes each step of the process step by step.

for example lets create a program that adds a value to all the numbers in an array.

```
var arr = [1,2,3,4,5];
var value = 2;
for (var i = 0; i < arr.length; i++) {
  arr[i] = arr[i] + value
}
console.log(arr) // [ 3, 4, 5, 6, 7 ]
```

This is imperative way to write this program. Everything is written in a step by step matter. You wouldn't inherently understand what the code is trying to do unless you read through the code base

## Declarative Programing

Now lets take the same example and write it in a declarative way.

```
var arr = [1,2,3,4,5];
console.log(arr.map(function(n) {return n + 2}));
```

In this example, we are using the map method to return a new array with the changes we defined. Notice that we have abstracted away the imperative way of writing this function, e.a. the step by step process of looping over the array, but instead represented it with a single `map` method.

You wouldn't inherently understand this code unless you knew what map did, however, you can make a good guess at what the `map` method is doing by just reading it the code. Someone reading this code for the first time could guess that `map` stands for mapping over the array, which is a common practice in programming.

This is essentially the point of declarative programming. It is the process where we abstract away the actual imperative function and wrap it around something that is more readably.

take the following example

```
var arr = [1,2,3,4,5];

var final = arr.map(function(n) {return n + 2}) // [ 3, 4, 5, 6, 7 ]
  .filter(function(n) {return n % 2 === 0}) // [4, 6]
  .reduce(function(a, b) {return a * b}) // 24

console.log(final);
```

Here we are mapping over the array and adding 2 two each value, then we are filtering just the even numbers, and finally we are multiplying the filtered even numbers to one value.

Writing this code in a declarative way allows us to logically see what the program is doing, without having to decipher the imperative steps behind the abstractions. This chaining of functions is called composing and it is a awesome way to write clear declarative code.

## The Cost

Remember that in programming there are always tradeoffs that you need to consider, and composing your code like the example above is also pray to this fact.

While our code is elegant and it logically makes sense to `map` the array then `filter` the array, then finally `multiply` the array in separate steps, this inherently creates inefficiencies in our code. For each call that we make, we are creating a intermediate array, also remember that in these methods we are passing a higher order function which means a function call is being made for every iteration. This can get very expensive for large data sets.

However, don't worry too much about this, as modern day computers are incredible fast and it will be rare that you noticed a major difference due to this cost. Just keep this in knowledge in the back of your mind for future reference.

## Challenge

### Higher Order By

```
function canEnterBar(validation) {
  if (validation) {
    console.log('you may enter the bar');
  } else {
    console.log('go home');
  }
}

function isOfAge(age) {
  // writer your code here
}

// 1. using a higher order function log the "you may enter the bar" log
// 2. using a higher order function log the "go home" log
```

### Refactor the imperative program

```
// refactor the below code in a declartive way

var programmers = [
  {
    name: 'john',
    language: 'javascript',
    age: 28,
    sex: 'male'
  },
  {
    name: 'tresha',
    language: 'python',
    age: 20,
    sex: 'female'
  },
  {
    name: 'jessica',
    langage: 'golang',
    age: 24,
    sex: 'female'
  }
];
console.log('******************************')
console.log('initial value', programmers);
// 1. filter out only the female programmers
var femalesOnly = [];
for (var j = 0; j < programmers.length; j++) {
  if (programmers[j].sex === 'female' ) {
    femalesOnly.push(programmers[j]);
  }
}
console.log('******************************')
console.log('female only programmers', femalesOnly);
// 2. add one to each female programmers age
for (var i = 0; i < femalesOnly.length; i++) {
  femalesOnly[i].age += 1;
}
console.log('******************************')
console.log('after adding to age', femalesOnly);

// 3. sum up their combined age
var totaleAgeFemale = 0;
for (var k = 0; k < femalesOnly.length; k++) {
  totaleAgeFemale += femalesOnly[k].age;
}
console.log('******************************')
console.log('total age of female programmers', totaleAgeFemale) // 46;
```

### ChainOrama

Taking the previous problem, e.a. Refactor the imperative program
can you filter out just the female engineers, add one to each of their age,
and finally sum up their age in one composition?
