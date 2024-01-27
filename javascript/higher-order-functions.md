#### What is it?
Higher order functions are those functions which take function as an argument or returns a function as the result.

#### How does it work?
This is possible because in JavaScript functions are treated as a first class citizens, which means that they can be treated like a value.

So, 
we can assign them to another variable
```
function test(){
	console.log("Hello World")
}

// assign it to a variable
const x = test;

// we can call x as the function
x();
// output 
// Hello world

```

 pass it as an argument
```
 function test2(anotherFunction){
	 console.log("hello from test 2");
	 // calling callback function
	 anotherFunction()
 }
 
 // passing function from above as an argument
 test2(test);
 
// output
// hello from test 2
// Hello world
```

returned as a value
```
function parentFunction(){
	function childFunction(){
		console.log("hello from child function")
	}
	return childFunction();
}

// child variable gets assigned the returned funtion
const child = parentFunction();
child();

// output
// hello from child function
```

#### Let's see how this concept(Higher order functions) helps us write cleaner code.

say we are provided an array of numbers and we have to perform some operation on each number and return an array of result.

let's see the function without the use of Higher order function:
```
// Without higher-order function

// operation one
function sumOfSquares(arr) {
  let sums = [];
  for (let i = 0; i < arr.length; i++) {
    sums[i] = arr[i] * arr[i];
  }
  return sums;
}

// operation two
function sumOfCubes(arr) {
  let sums = [];
  for (let i = 0; i < arr.length; i++) {
    sums[i] = arr[i] * arr[i] * arr[i];
  }
  return sums;
}

// array of numbers
const numbers = [1, 2, 3, 4, 5];

// Calculate sum of squares
const squaresSum = sumOfSquares(numbers);

// Calculate sum of cubes
const cubesSum = sumOfCubes(numbers);

// Output
console.log(squaresSum); // Output: [ 1, 4, 9, 16, 25 ]
console.log(cubesSum); // Output: [ 1, 8, 27, 64, 125 ]

```

As you can notice on the above code we have written a lot of repetitive code, the only difference is of the logic.

This is where Higher order functions comes into action, we can write a much cleaner code with it, let's break down how we are going to achieve it
- Create a generic function that handles common tasks.
- Create separate functions(logic function) that handles one task.
- Call the generic function with array and the logic function as it's arguments.


Generic function
```
function calculateByOperation(arr, operation) {
  const resultArray = [];
  for (let i = 0; i < arr.length; i++) {
    resultArray.push(operation(arr[i]));
  }
  return resultArray;
}
```


Logic functions
```
function square(x) {
  return x * x;
}

function cube(x) {
  return x * x * x;
}

function double(x) {
  return x * 2;
}
```

Calling generic functions
```
const numbers = [1, 2, 3, 4, 5];

// Calculate array of squares using the higher-order function
const resultWithSquare = calculateByOperation(numbers, square);

// Calculate array of cubes using the higher-order function
const resultWithCube = calculateByOperation(numbers, cube);

// Calculate array of doubled values using the higher-order function
const resultWithDouble = calculateByOperation(numbers, double);

// Output
console.log(resultWithSquare); // Output: [1, 4, 9, 16, 25]
console.log(resultWithCube);   // Output: [1, 8, 27, 64, 125]
console.log(resultWithDouble); // Output: [2, 4, 6, 8, 10]
```


#### Benefits of writing code like this
- By separating generic functions you can reuse your code for other tasks, where you need similar template.
- By abstracting away specific logic into a separate function, you create a clearer distinction between the core functionality and how it behaves on different inputs.
- Each function can focus on a single task, which makes it easier to understand, maintain and make changes. Making changes to one part of the code won't necessarily affect other.
