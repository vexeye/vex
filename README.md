# vexx

## Functions

All functions receive only one parameter. 

This is enforced for various reasons.

- Named Parameters: Using a parameter object allows you to use named parameters, which can make your code more self-explanatory and less error-prone. Instead of relying on the order of parameters, developers can specify the parameter they want to set explicitly.
- Flexibility: With a parameter object, you can easily extend the function's parameters without changing the function signature. This makes it more adaptable to future changes or additional parameters.
- Default Values: You can provide default values for parameters within the object, so if a parameter is not specified, the function can fall back to a predefined value.
- Clarity: When functions have multiple parameters, each with its meaning, it can be challenging to remember the order or purpose of each parameter. A parameter object provides a clear and structured way to pass and access parameters.
- Optional Parameters: It's easy to make parameters optional by not including them in the object if they are not needed for a particular call. This can simplify function calls.
- Reduced Function Signature Complexity: Using a single parameter object can help keep the function signature simple, especially when there are many parameters.

All of this with zero runtime overhead

```
// Definition
fn name(p: ParamType) -> ReturnType {
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

## Blueprints

```
bp BlueprintName(&self) {
# public
    field1: FieldType1
    field2: FieldType2
    // ...

    constructor(p: BlueprintNameProps) {
        // Constructor implementation
    }

    fn method1(p1: ParamType1) -> ReturnType1 {
        // Method implementation
    }

    fn method2(p2: ParamType2) -> ReturnType2 {
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
# @Inherited
    bankAccount: String
# @Hidden
    skeletsInTheCloset: String
}
```

### Coupling

In Vexx there are two ways of coupling.

- Strict via inheritance
- Loose via composition

#### Inheritance

Inheritance is done by using the `:` symbol between the `Child` and the `Parent(s)`

> ℹ️ When using inheritance, all the properties of the parents would be accessible or in other words, merged, in the `&self` reference.

```
// Single parent
bp Child(&self) : Parent {
    fieldName: String

    constructor(props: BlueprintNameProps) {
        log`&self.bankAccount`
    }
}

// Many parents
bp Child(&self) : [Parent1, Parent2] {
    fieldName: String

    constructor(props: BlueprintNameProps) {
        log`&self.bankAccount`
    }
}
```

#### Composition

Composition is done by using the `~` symbol between the `Child` and the `Sibling(s)`

> ℹ️ When connected to a blueprint, the sibling object will be injected into the `&self` reference and the name would be the camelCase version of the blueprint itself.

```
// Single sibling
bp Child(&self) ~ Sibling {
    fieldName: String

    constructor(props: BlueprintNameProps) {
        log`&self.sibling.anotherField`
    }
}

// Many siblings
bp Child(&self) ~ [Sibling1, Sibling2] {
    fieldName: String

    constructor(props: BlueprintNameProps) {
        log`&self.sibling1.toy1`
        log`&self.sibling2.toy2`
    }
}

```

## Data types

```
fn main() {
    // Bool type
    let isTrue: Bool = true
    let isFalse: Bool = false

    // Byte type
    let byteValue: Byte = 42

    // Integer types
    let int8Value: Int8 = 127
    let int16Value: Int16 = 32767
    let int32Value: Int32 = 2147483647
    let int64Value: Int64 = 9223372036854775807

    // Char type with backtick delimiters
    let charValue: Char = `A`

    // String type with backtick delimiters
    let stringValue: String = `Hello, world!`

    // Symbol type with backtick delimiters
    let symbolValue: Symbol = @`Symbol`

    // Your code goes here
}
```

## Control flow
