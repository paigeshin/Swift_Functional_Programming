# Swift_Functional_Programming

# Functional Programming

Functional programming is a programming paradigm where **immutability** is favored. The problem with mutation is that you need to know every shared variable to know the effect of a change. Mutation can lead to unexpected side-effects. Functional Programming makes our code more predictable and less prone to issues like race-conditions.

# Functional Programming vs Object Oriented Programming

Object oriented programming makes code understandable by encapsulating moving parts. Functional programming makes code understandable by minimizing moving parts. 

# Functions as First Class Citizens

Functions in Swift are **first class citizens** meaning we can use them just like any other object or value. In general, first class citizens can:

- Be stored in a variable or constant
- Be passed as arguments to functions
- Be returned by functions

```swift
// Be stored in a variable
func getCourseName() -> String {
	return "App Design, UI/UX and Development"
}

let courseNameFunction = getCourseName 
courseNameFunction()

// Be passed as arguments to functions 
func course(name: () -> String) {
	name() 
}
course(name: getCourseName)

// Be returned by functions 
func getCourseNameFunction() -> () -> String {
	return getCourseName
}

getCourseNameFunction()()

// Stored in some kind of data structure 
let courseNameArray = [getCourseName, getCourseName, getCourseName]
for courseName in courseNameArray {
		print(courseName())
}

courseNameArray[0]()
```

# Closures as First Class Citizens

Closures are basically just functions without names. They are unique in that they are able to capture values from their surrounding context. Since closures are functions they behave as first class citizens.

```swift
// assigning to a constant or variable
func addNumbers(_ number1: Int, _ number2: Int) -> Int {
    return number1 + number2
}

print("add numbers in function result is \(addNumbers(2, 2))")

let addNumbersClosure: (Int, Int) -> Int = {(_ number1: Int, _ number2: Int) -> Int in
    return number1 + number2
}

let addNumbersClosure2: (Int, Int) -> Int = {(number1: Int, number2: Int) -> Int in
    return number1 + number2
}

print("add numbers in closulre result is \(addNumbersClosure(2, 2))")

// passing to a function
func add(numbers: (Int, Int) -> Int) {
    let result = numbers(3, 2)
    print(result)
}

add(numbers: addNumbersClosure)

// return from a function
func addNumbersFunction() -> (Int, Int) -> Int {
    return { (number1, number2) in
        return number1 + number2
    }
}

let addNumbers = addNumbersFunction()
let result = addNumbers(3, 3)
print(result)
```

# Closure Expressions

Closures have different types of expression syntax, which include:

### General Closure Expression

```swift
{ (parameter) -> (type) in 
	return value 
}
```

### Inferring Type From Context

```swift
{ paramter in 
	value 
}
```

### Implicit Returns (For Single Expression Closures)

```swift
{ paramters in 
	value 
}
```

### Shorthand  Argument Names

Parameter arguments are expressed in short hand e.g. $0, $1, $2 

### Trailing Closure Syntax

If you specify a closure expression as the last parameter in a function, you can write it in trailing closure syntax. The floowing:

```swift
func functionName(function parameters, closure expression) {
	statements 
}
```

# Closure Expression Examples

```swift
let addNumbersGeneralExpression = { (number1: Int, number2: Int) -> Int in
    return number1 + number2
}

let addNumbersInferredExpression: (Int, Int) -> Int = { (number1, number2) in
    return number1 + number2
}

let addNumbersInferredExpression2 = { (number1: Int, number2: Int) in
    return number1 + number2
}

let addNumbersImplicitReturnExpression = { (number1: Int, number2: Int) in
    number1 + number2
}

let addNumbersShorthandExpression: (Int, Int) -> Int = { $0 + $1 }

func add(numbers: (Int, Int) -> Int) {
    print(numbers(3,2))
}

// general expression
add { (num1, num2) -> Int in
    return num1 + num2
}

// inferred type
add { (num1, num2) in
    return num1 + num2
}

// implicit return
add { (num1, num2) in
    num1 + num2
}

// shorthand argument type
add { $0 + $1 }

// Another Example
func getFullNmae(firstName: String, lastName: String, result: (String) -> Void) {
    let fullName = firstName + " " + lastName
    result(fullName)
}

getFullNmae(firstName: "Steve", lastName: "Jobs") { fullName in
    print(fullName)
}
```

# Escaping Closures

An escaping closure is any closure that is passed as an argument to a function that needs to outlive the function. In other words, **an escaping closure executes after the function returns** and therefore **needs to live longer than its function.** This is needed in situations where the closure handles a lengthy task such as downloading content from a server. By default, closures passed as arguments are non-escaping. Closures escape if the @escaping keyword is used.  

# Higher Order Functions

“In mathematics and computer science, a higher-order function is a function that does at least one of the following: takes one or more functions as arguments, returns a function as its result. All other functions are first-order functions.”

- map - transforms each element of an array
- filter - only returns the elements of an array that pass a certain condition
- reduce - combines all the elements of an array into a single value
- mapValues - transforms only the values of a dictionary
- compactMap - removes all elements in dictionary that are nil

```swift
let numberArray = [2, 4, 7, 5, 1, 9]
let simpleDictionary: [String: String?] = ["a": "1", "b": nil, "c":  "3"]
let arrayOfStrings = ["Car", "Boat", "Plane", "Building"]

// map example
print(arrayOfStrings.map { $0.count })

// filter example
print(numberArray.filter { $0.isMultiple(of: 2) })

// reduce example
print(numberArray.reduce(0) { $0 + $1 })
      
// mapValues example
print(simpleDictionary.filter{ $0.value != nil }.mapValues{ $0! })

// compactMap example
print(simpleDictionary.compactMap{ $0 }
```

# Functional Programming In Practice

Functional programing is a programming paradigm where code is created using pure functions and where mutability is avoided. Swift is not a functional programming language but we can borrow functional programming principles to improve our code. Benefits inlcude:

- Code is easier to test
- Ease of debugging
- Cleaner code
- Less Bugs

### Pure Functions

1. A pure function always returns the same output given the same input.
2. Has no side-effects

### Higher Order Functions

Higher-order functions allow us to abstract over actions, not just values. Higher order functions are a utility all functional programming languages have that allow us to process data in an isolated way with no side-effects. 

### Declarative

Imperative code describes the steps needed to achieve a certain result. Functional code abstract the flow process and instead describes the data flow or the “what to do”

### Functional Programming Examples

```swift
/// Example 1
var newResult = 0

func notPureFunction(num1: Int, num2: Int) {
    newResult = num1 + num2
}

func pureFunction(num1: Int, num2: Int) -> Int {
    return num1 + num2
}

/// Example2
let studentScores = [3, 4, 1, 7, 2, 10, 5]

func nonFunctionalStudentScoreFilter(_ scores: [Int]) -> [Int] {
    var bestScores = [Int]()
    for score in scores {
        if score > 5 {
            bestScores.append(score)
        }
    }
    return bestScores
}

func functinalStudentScoreFilter(_ scores: [Int]) -> [Int] {
    return scores.filter { $0 > 5 }
}

/// Example3
let locationTemps = ["South Africa": 23, "Japan": 10, "Greece": 15, "Egypt": 25]

struct WarmCity {
    var cityName: String
    var temp: Int
}

var warmCities = [WarmCity]()

// Non Functional Way
for locationTemp in locationTemps {
    if locationTemp.value > 16 {
        warmCities.append(WarmCity(cityName: locationTemp.key, temp: locationTemp.value))
    }
}

// Functional Way
locationTemps.filter { $0.value > 16 }.map { (city, temp) -> WarmCity in
    WarmCity(cityName: city, temp: temp)
}
```
