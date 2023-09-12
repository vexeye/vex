# vexx

## Data types

```
fn main() {
    // 1. Bool type
    let bFalse: Bool = true
    let bFalse: Bool = false

    // 2. Numbers
    let v = 1
    let value: Number = 1
    let bitValue: Bit = 1
    let int8Value: I8 = 127
    let int16Value: I16 = 32767
    let int32Value: I32 = 2147483647
    let int64Value: I64 = // ...
    let int128Value: I64 = 9223372036854775807

    // Unsigned
    let unsignedInt8Value: uI8 = 256
    let unsignedInt16Value: uI16 = 65536
    let unsignedInt32Value: uI32 = 4294967294
    let unsignedInt64Value: uI64 = 9223372036854775807

    // Floats
    let int8Value: F16 = 1.0  // 16-bit floating point (10-bit mantissa) IEEE-754-2008 binary16
    let int8Value: F32 = 1.0  // 32-bit floating point (23-bit mantissa) IEEE-754-2008 binary32
    let int8Value: F64 = 1.0  // 64-bit floating point (52-bit mantissa) IEEE-754-2008 binary64
    let int8Value: F80 = 1.0  // 80-bit floating point (64-bit mantissa) IEEE-754-2008 80-bit extended precision
    let int8Value: F128 = 1.0 // 128-bit floating point (112-bit mantissa) IEEE-754-2008 binary128

    // Special
    let numberInRange: Number({ range: 1...500 }) = 499

    // 3. Characters

    // Char type with backtick delimiters
    let charValue: Char = `A`

    // String type with backtick delimiters
    let stringValue: String = `Hello, world!`

    // Symbol type with backtick delimiters
    let symbolValue: Symbol = @`Symbol`

    // 4. Unexpected behavior
    let null: Null
    let void: Void
    let error: Error
    let exception: Exception

    // 5. Generics
    let type: Type

    // Your code goes here
}
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

### General purpose functions

#### Lamda functions

Anonymous lamda functions are short-lived functions that can be defined only under a certain context, such as a callback for another function.

```
object.map((p: T): ReturnType => {
  // Function body
})
```

#### Named functions

```
// Declaration & Definition
fn example(p: T): ReturnType => {
  // Function body
}

// Usage
example({})
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

### Access modifiers

Access modifiers with zero runtime overhead, compile time memory alignment and performance optimizations.

By design there are 2 ways of looking at access modifiers.

- > Implicit - All fields and methods are public my default
- > Explicit - Granularly group related fields in a vertical manner

```
// Implcit
bp Parent(&self) {
// All fields are public
    fullName: String
    bankAccount: String
    skeletsInTheCloset: String
}

// Explicit
bp Parent(&self) {
// Grouped with visibility decorator
# @Visible
    fullName: String
# @Inherit
    bankAccount: String
# @Hidden
    skeletsInTheCloset: String
}
```

### Coupling

In Vexx there are two ways of coupling.

- Strict via blueprint inheritance
- Loose via dependency injection

#### Inheritance

Inheritance is done by using the `:` symbol between the `Child` and the `Parent(s)`

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
bp Child(&self) : [Parent1, Parent2] {
    fieldName: String

    constructor(props: ChildProps) {
        log`&self.bankAccount`
    }
}
```

#### Dependency injection

Dependency injection is done by using the `~` symbol between the `Child` and the `Sibling(s)`

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
