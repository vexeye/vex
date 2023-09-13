# vexx

## Hello world

```
// main.x
say`Hello, World!`
```

## Entrypoint priority

The priority for the entrypoint of an application or library goes as follows:

1. `main.x`
2. An `fn main() -> {}` impure function in any file with this name

- Multiple `main.x` files are prohibited per one single application, library.
- Multiple `main` functions are prohibited per one single application, library.

## Literal language

Vexx is a highly literal language with an emphasis on keeping the balance between readability, consistency and developer experience & ergonomics without sacrificing compilation or runtime speed of the produced applications & libraries.

### Comments

#### Inline

```
// This is an inline comment.

// ! This is a highly important inline comment.

// ? This is an optional, question-like comment.

// @Decorator can be used in an inline comment to enchance it.

/// This is comment that will be available in the generated docs.
```

#### Multi-line

```
/**
* This is a multi-line comment.
*/

/**
* This is a multi-line comment with a @vexx/doc decorator.
* @See https://vexxlang.com
*/

/**
* This is a chonky multi-line comment with multiple @vexx/doc decorators.
* @Author Reanimated Man X <mail@rmx.codes> / Alexei Gaidulean
* @See https://vexxlang.com
*/
```

### Comment section

Section comments are use to group related code together for easier, digestible reading.

Section comments can be then collapsed/expanded by the IDE, while simple comments can not.

```
// any.x

# Title commentary

## Sub-section commentary

fn doSomething(): T => {
  // Implementation
}

# Title commentary 2

## Sub-section commentary 2

fn doSomethingNasty(): T => {
  // Implementation
}
```

## Data types

## Logical

```
let bFalse: Bool = true
let bFalse: Bool = false

```

## Numbers

```
// Real
let v = 1 // Number
let value: Number = 1
let bitValue: Bit = 1

let int8Value: I8 = 127
let unsignedInt8Value: uI8 = 256

let int16Value: I16 = 32767
let unsignedInt16Value: uI16 = 65536

let int32Value: I32 = 2147483647
let unsignedInt32Value: uI32 = 4294967294

let int64Value: I64 = // ...
let unsignedInt64Value: uI64 =// ...

let int128Value: I128 = // ...
let unsignedInt128Value: I128 = // ...

// Floats
let int8Value: F16 = 1.0  // 16-bit floating point (10-bit mantissa) IEEE-754-2008 binary16
let int8Value: F32 = 1.0  // 32-bit floating point (23-bit mantissa) IEEE-754-2008 binary32
let int8Value: F64 = 1.0  // 64-bit floating point (52-bit mantissa) IEEE-754-2008 binary64
let int8Value: F80 = 1.0  // 80-bit floating point (64-bit mantissa) IEEE-754-2008 80-bit extended precision
let int8Value: F128 = 1.0 // 128-bit floating point (112-bit mantissa) IEEE-754-2008 binary128

// Special
let numberInRange: Number({ range: 1...500 }) = 499
```

## Characters

```
// Char type with backtick delimiters
let charValue: Char = `A`

// String type with backtick delimiters
let stringValue: String = `Hello, world!`

// Symbol type with backtick delimiters
let symbolValue: Symbol = @`Symbol`
```

## Special cases

```
let void: Void
let null: Null
let unknown: Unknown

let required: Required
let optional: Optional

let composable: Composable
let returnable: Returnable
let unreturnable: Unreturnable
```

## Unexpected behavior

```
// Recoverable unexpected behavior
let exception: Exception

// Unrecoverable unexpected behavior
let error: Error
```



## Generic types, traits

```
let type: Type
```


## Functions

All functions receive only one parameter. 

This is enforced for various reasons.

- Named Parameters: Using a parameter object allows you to use named parameters, which can make your code more self-explanatory and less error-prone. Instead of relying on the order of parameters, developers can specify the parameter they want to set explicitly.
- Flexibility: With a parameter object, you can easily extend the function's parameters without changing the function signature. This makes it more adaptable to future changes or additional parameters.
- Default Values: You can provide default values for parameters within the object, so if a parameter is not specified, the function can fall back to a predefined value.
- Clarity: When functions have multiple parameters, each with its meaning, it can be challenging to remember the order or purpose of each parameter. A parameter object provides a clear and structured way to pass and access parameters.
- Optional Parameters: It's easy to make parameters optional by not including them in the object if they are not needed for a particular call. This can simplify function calls.
- Reduced Function Signature Complexity: Using a single parameter object can help keep the function signature simple, especially when there are many parameters.

All of this with zero runtime overhead.

```
// Definition
fn name(p: ParamType) => ReturnType {
    // Function body
    // ...
    return returnValue
}

// Call
name({
  id: `1234`
  value: `Value`
})
```

### Syncronous functions

#### Pure functions

Pure functions cannot call other non-pure functions inside it.

```
// Declaration & Definition
fn example(p: T): ReturnType => {
  let n = getSomethingPure()
  // Do something with n
  
  // Function body
  return ReturnType
}

// Usage
example({})
```

#### Impure functions

Impure functions can call both pure & impure functions inside it, and are assumed to contain side effects.

```
fn example(p: T): ReturnType | Void -> {
    let something = getSomethingPure()
    processImpure(&something)
}
```

#### Generator functions

Generator functions are used to yield results generatively, whenever they are needed.

```
fn infinite(p: T): ReturnType /> {
  let n = 0
  loop {
    yield n
    n = F.increment(n)
  }
}

log`$infinite().next()` // 0
log`$infinite().next()` // 1
```


### Asynchronous functions

Async functions return a promise to be delivered, when called, they are not blocking the further execution no matter how long the main thread has to await for the result to come.

#### Continuous async function
```
// Declaration & Definition
fn example(p: T): ReturnType |> {
  // Function body
}

// Usage
await example()
```

### Pausable / Cancellable async function

```
// Declaration & Definition
fn example(p: T): ReturnType ||> {
  // Function body
}

// Usage
const promise = await example().next()

await promise.next()
await promise.pause()
await promise.cancel()
```

### General purpose functions

#### Lamda functions

Anonymous lamda functions are short-lived functions that are bound to a callback of another function, and cannot be declared somewhere else.

They can be one of types:
- Syncronous
- Asynchronous

```
// Syncronous
object.map((p: T): ReturnType => {
  // Function body
})

// Asynchronous
object.map((p: T): ReturnType |> {
  // Function body
})
```

#### Function constructor

The `fn` method is an alias for `F` blueprint primitive that is used for all functions.

```
// Log an empty pure function
log`$F` 

// Create and call a pure function, returns void result
log`$F()`

// The `F` constructor has many statically available functional methods
F::pipe()
F::compose()
F::converge()

F::join()
F::merge()

```

## Decorators

Decorators are functional blueprints to vertically extend other building blocks.

```
dc DecoratorName(&self) {
    let new: T = `Something`

    constructor(props: DecoratorNameProps) {
       // Do something
    }
}

@DecoratorName({
    bColoredLog: true
})
fn someFunction() => {
    // Implementation
}

@DecoratorName({
   bColoredLog: true
})
bp BlueprintName(&self) {
    // Implementation
}
```

## Blueprints

```
bp BlueprintName(&self) {
    field1: FieldType1
    field2: FieldType2
    // ...

    constructor(p: BlueprintNameProps) {
        // Constructor implementation
    }

    fn method1(p1: ParamType1) => ReturnType1 {
        // Method implementation
    }

    fn method2(p2: ParamType2) => ReturnType2 {
        // Method implementation
    }
}
```

### Methods

Methods are functions bound to a blueprint scope.

There can be two types of methods:

#### Bound

```
// Declaration & Definition
bp BlueprintName(&self) {
    fn method(p1: ParamType1) => ReturnType { // 👈 This is a bound method as it's a direct child of the owning `bp`
        // Method implementation
        return ReturnType
    }
}

// Usage
let b = BlueprintName()

b.method()
```

#### Static

Static methods can be called directly from the blueprint avoiding the need to instantiate it, they cannot access the `&self` reference inside them.

```
bp BlueprintName(&self) {
# @Static
    fn method(p1: ParamType1) => ReturnType { // 👈 This is static method declared in the `Static` section `bp`
        // Method implementation
        return ReturnType
    }
}

// Usage
BlueprintName::method() // 👈 The call to a static method is done via `::`
```

### Access modifiers

Access modifiers with zero runtime overhead, compile time memory alignment and performance optimizations.

By design there are 2 ways of looking at access modifiers.

#### Implcit

All fields and methods are visible to the consumers by default
```
bp Parent(&self) {
    fullName: String
    bankCredentials: String
    skeletsInTheCloset: String
}
```

#### Explicit

Fields and methods are granularly grouped in a vertical manner

```
bp Parent(&self) {
# @Visible
    fullName: String
# @Inherit
    bankCredentials: String
# @Hidden
    skeletsInTheCloset: String
}
```

### Coupling

In Vexx there are two ways of coupling.

- Strict via blueprint parent inheritance
- Loose via blueprint dependency composition

#### Dependency Inheritance

Dependency Inheritance is done by using the `:` symbol between the `Child` and the `Parent(s)`

> ℹ️ When using inheritance, all the properties of the parents would be accessible or in other words, merged, in the `&self` reference.

```
// Single parent
bp Child(&self) : Parent {
    fieldName: String

    constructor(props: ChildProps) {
        log`&self.bankAccount`
    }
}

// Many parents
bp Child(&self) : [Father, Mother] {
    name: String

    constructor(props: ChildProps) {
        log`&self.maleTraits`
        log`&self.femaleTraits`
    }
}

bp Child(&self) : [Parent1, Parent2, Parent3] {
    name: String

    constructor(props: ChildProps) {
        log`&self.uniqueField1`
        log`&self.uniqueField2`
        log`&self.uniqueField3`
    }
}
```

#### Dependency Composition

Dependency composition is done by using the `~` symbol between the `Child` and the `Sibling(s)`

> ℹ️ When connected to a blueprint, the sibling object will be injected into the `&self` reference and the name would be the camelCase version of the blueprint itself.

```
// Single sibling
bp Child(&self) ~ Sibling {
    fieldName: String

    constructor(props: ChildProps) {
        log`&self.sibling.anotherField`
    }
}

// Many siblings
bp Child(&self) ~ [Sibling1, Sibling2] {
    fieldName: String

    constructor(props: ChildProps) {
        log`&self.sibling1.toy1`
        log`&self.sibling2.toy2`
    }
}

```


## Control flow
