# vex

vex, a general purpose universal code language with an emphasis on code literacy, readability, maintainability, consistency and developer experience. All this while keeping up with top priorities such compilation and runtime performance of the produced applications & libraries.

![code](https://github.com/vexeye/vex/assets/32410574/59d6e8d3-a8f6-4e4d-9a5c-b3e39262dc6f)

## Getting started

1. Install `vex` cli
2. Run `xx new app`

You should get a directory like so:

```
src
  main.x
test
  benchmark
    .keep
  e2e
    .keep
  spec
    .keep
package.x
README.md
```

## Hello world

```vex
// main.x
say`Hello, world!`
```

## Manifesto

```text
1.
1.1. Documentation is essential.
1.2. Benchmarking is pivotal.
1.3. Testing is fundamental.
1.4. Readability is crucial.
1.5. Consistency is vital.

2.
2.1. Beautiful is better than ugly.
2.2. Shorter is better than longer.
2.3. Sparse is better than dense.
2.4. Flat is better than nested.
2.5. Nested is better than mess.

3.
3.1 Complex is better than complicated.
3.2 Simple is better than complex.
3.4 Clear is better than ambiguous.
3.3 Explicit is better than implicit.
3.5 Descriptive is better than vague.

4.
4.1. Developer experience over unnecessary struggle.
4.2. Quantity of files over quantity of lines.
4.3. Reusability over repetitiveness.
4.4. Verticality over horizontality.
4.5. Patterns over randomness.

5.
5.1 In the face of ambiguity, refuse the temptation to guess.
5.2 Special cases aren't special enough to break the rules.
5.3 Historical benchmarking over premature optimization.
5.4 Errors should never be logged superficially.
5.5 Exceptions should never pass silently.
```

## Entrypoint priority

The priority for the entrypoint of an application or library goes as follows:

1. `main.x`
2. An `fn main() -> {}` impure function in any file with this name

- Multiple `main.x` files are prohibited per one single application, library.
- Multiple `main` functions are prohibited per one single application, library.

## Literal language

Being a highly literal language it is important to have everything tool and building block to the disposal of the developer, enabling the writing of clear, readable and maintainable code.

### Comments

#### Inline

```vex
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
* This is a multi-line comment with a @vex/doc decorator.
* @See https://vexlang.com
*/

/**
* This is a chonky multi-line comment with multiple @vex/doc decorators.
* @Author Reanimated Man X <mail@rmx.codes> / Alexei Gaidulean
* @See https://vexlang.com
*/
```

### Comment sections

Comment sections are used to group related code together for easier, digestible reading.

Comment sections can be then collapsed/expanded by the IDE, while simple comments can not.

```
// any.x

# Title commentary

## Sub-section commentary

fn doSomething(): T => {
  # Process this thing
  // Implementation

  # Process that thing
  // Implementation
}

# Title commentary 2

## Sub-section commentary 2

fn doSomethingAwesome(): T => {
  // Implementation
}
```

### Comment Regions

Comment sections are great to vertically split code in meanignfull sections, or "chapters" to list through, however it's not possible to group or encapsulate code in them as all the code behind a section will always be part of it.

To add an encapsulating comment region without adding an extra level of identation:

```vex
fn doSomething(): T => {
  /# Unspeakable.
  say`Anything!`
  /#

  # Do something else
  // Implementation
}
```

### Variable monospace fonts

```
TBD
```

### Ligatures as first class citezen

```
TBD
```

## Printing, logging & debugging

vex has all the functionality to print to the terminal using `7` different log levels for general purpose use.

### General purpose

```
say`Hello, world! 👋`

log`Server started at $address:$port.`
info`Server database seeding succeeded.`
warn`Server database schema is inconsistent.`

alert`Server received unexpected traffic from $ip. Notifying...`
panic`Server database connection failed: $exception. Retrying...`
fatal`Service encountered an unrecoverable error: $error. Exiting...`
```

### Tracing, benchmarking and debugging

```
dbg(obj)
trace(obj)

time`Long running function`
timeLog`Long running function`

profile`Test case: CPU Spikes`
profileLog`Test case: CPU Spikes`
```

### Utility

```
table([{a: 1}, {a: 2])
group`This is a logging group`
groupEnd`This is a logging group`
```

## Foundations

vex is structured and build around repetitive, consistent structures called **blocks**.

### Anatomy

Here is a general overview of vex block anatomy.

```
// Block
  👇 Header token
  {   👈 Block header line
      👈 Block body line(s)
  }   👈 Block footer line
 👆 Footer token
```

### Block tokens

There can be `3` types of blocks

```
// "Encapsulation" block, represented by `{` and `}` characters.
{
   // Body
}

// "Enumeration" block, represented by `[` and `]` characters.
[
   // Body
]

// "Expression" block, represented by `(` and `)` characters.
(
   // Body
)

// Example

bp Blueprint {
  // Body
}

en Enum [
  A
  B
]

compose Compose (
  Fn1
  Fn2
  Fn3
)
```

### Semantics

```
// Anonymous block
{

}

// Unique anonymous block
token {
     // Body
}

// Unique anonymous block with callable expression
token (expression) {
     // Body
}

// Uniquely named block
token Example {
     // Body
}

// Uniquely named block with callable expression
token Example(expression) {
     // Body
}

// Uniquely named block with callable expression and header extension.
token Example(expression) token value {
     // Body
}

// Uniquely named block with callable expression and header and footer extension.
token Example(expression) token value {
     // Body
} token value
```

There are `7` block categories.

- Extension Blocks
- Modeling Blocks
- Building Blocks
- Grouping Blocks
- Storage Blocks
- Logical Blocks
- Loop Blocks

```
TBD
```

## Primitives

vex provides access to a wide variety of primitives.

### Scalar Types

- Bitwise types: Bit
- Signed integers: I8, I16, I32, I64, I128 and ISize (pointer size)
- Unsigned integers: U8, U16, U32, U64, U128 and USize (pointer size)
- Floating point: F32, F64
- Char Unicode scalar values like 'a', 'α' and '∞' (4 bytes each)
- Bool either true or false
- The Unit type (), whose only possible value is an empty tuple: ()

> ℹ️ Despite the value of a `Unit` type being a tuple, it is not considered a compound type because it does not contain multiple values.

#### Bit

```
let bitValue: Bit = 1 // 0 or 1
```

#### Signed integer

```
let int8Value: I8
let int16Value: I16
let int32Value: I32
let int64Value: I64
let int128Value: I128
```

#### Unsigned integer

```
let unsignedInt8Value: uI8
let unsignedInt16Value: uI16
let unsignedInt32Value: uI32
let unsignedInt64Value: uI64
let unsignedInt128Value: uI128
```

#### Floating point

```
let int8Value: F16 = 1.0  // 16-bit floating point (10-bit mantissa) IEEE-754-2008 binary16
let int8Value: F32 = 1.0  // 32-bit floating point (23-bit mantissa) IEEE-754-2008 binary32
let int8Value: F64 = 1.0  // 64-bit floating point (52-bit mantissa) IEEE-754-2008 binary64
let int8Value: F80 = 1.0  // 80-bit floating point (64-bit mantissa) IEEE-754-2008 80-bit extended precision
let int8Value: F128 = 1.0 // 128-bit floating point (112-bit mantissa) IEEE-754-2008 binary128
```

#### Infered numbers

Any of the above can be infered via the `Number` type

```
let v = 2         // Implicitly Number
let v = 2: Number // Explicitly Number

// Special
let numberInRange: Number({ range: 1...500 }) = 499
```

#### Logical

```
let bFalse: Bool = true
let bFalse: Bool = false
```

#### Characters

```
// Char type with backtick delimiters
let charValue: Char = `A`

// String type with backtick delimiters
let stringValue: String = `Hello, world!`

// Symbol type with backtick delimiters
let symbolValue: Symbol = @`Symbol`
```

#### Special cases

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

#### Unexpected behavior

```
// Recoverable unexpected behavior
let exception: Exception

// Unrecoverable unexpected behavior
let error: Error
```

#### Compound Types

- Arrays

```
[1, 2, 3]
```

- Tuples like

```
(1, true)
```

### Literals

Integers 1, floats 1.2, characters 'a', strings "abc", booleans true and the unit type () can be expressed using literals.

Literal modifiers are single characters prefixed, suffixed or part of the literal body.

Below is a list of literal modifiers.

#### Hexadecimal

```
0x

0x80
```

#### Octal

```
0o

```

#### Binary

```
0b

0b0011
```

#### Scientific E-notation (F64)

```
1e6
7.6e-4
```

#### Number blank separator

```
100000000 is the same as `10_000_000`
0.0000001 is the same as 0.000_001
```

### Operators

#### And

```
true && true
```

#### Or

```
true || false
```

#### Not

```
!true
```

#### Range

```
1 .. 5
```

TBD Bitwise operators

## Control flow

### Few conditions

`assert` control block is used for short, non-exhaustive, simple to digest assertions

- `assert` blocks cannot be nested
- `assert` blocks do not have `assert if` statements

```
// Assert guards
assert !isReady {
    return
}

// Multiple conditions
// ℹ️ Up to 3 distinct chained conditions in expression
assert isUserInvited || isUserCreated || isUserOnboarded  {
    say`🍾 Congrats`
}

// Call a function and return the flow to the block parent
assert value == 1 {
    doThis()
    return
}

// Add an `else` fallback
assert value == 3 {
    say`The value is nuts!`
} else {
    warn`But something doesn't add up`
}

// Use the expression value locally
assert value == 3 as Some(v) {
    say`The value of $v is total nuts!`
} else {
    warn`But something doesn't add up`
}

```

> ℹ️ For complex expressions involving many conditions use `match`

### Many conditions

```
// Match single value against expressions
match value {
    1       -> doThis()
    2       -> doThat()
    6 || 9  -> doChoose()
    3 .. 5  -> doInRange()
    _       -> doFallback()
}

// Match all values against expressions
match * {
    value1 = 1             -> doThis()
    value2 = 2 && isMirror -> doThat()
    _                      -> doFallback()
}

// Pass the matched value
match value {
    1      (v) -> doThis(v)
    2      (v) -> doThat(v)
    _      (v) -> doFallback(v)
}

// Use block scope for calling multiple impure functions
match value {
    1 (v) -> {
      callSync(v)
      await callAsync(v)
    }
    2 () -> doThat()
    _ () -> doFallback()
}

// Define a matching strategy for functions with identical input parameters
fn do = match {
    1 : doThis
    2 : doThat
    _ : doFallback
}

// Usage
do(value)
```

## Generics

```
let type: Type
```

## Logical expressions

```
TBD
```

## Advanced CLI sub-commands

`vex` comes with a CLI to do all sorts of things.

### Package management

```bash
xx i
xx add <@scope/package>
xx upd <@scope/package>
xx rm <@scope/package>
```

### Software development

**Running & Building**
```bash
xx dev
xx start
xx build
```

**Documenting & Testing**
```bash
xx docs
xx bench
xx test
```

**Versioning & Distributing**
```bash
xx tag
xx pack
xx version
xx publish
xx release
```

## Packages

vex enforces the user to declare & define a single building block entity per file, unless part of a namespace, therefore learning how to import/export building blocks is an important step moving forward.

### Using the installed packages

**1. Use standart sources / packages.**
```
// Use everything.
use fmt

// Use everything at path.
use fmt/deep/internals

// Use everything as a local alias.
use fmt as Alias

// Use only a part.
use { Part } from fmt

// Use only a part as a local alias.
use { Part as Alias } from fmt

// Use only a subset.
use fmt:namespace

// Use only a part of a subset.
use { Part } from fmt:namespace

// Use only a part of a subset as a local alias.
use { Part as Alias } from fmt:namespace
```

**2. Use internal project sources / packages. (relative)**
```
// Use everything.
use ./relative

// Use everything at path.
use ./relative/deep/path

// Use everything as a local alias.
use ./relative as Alias

// Use only a part.
use { Part } from ./relative

// Use only a part as a local alias.
use { Part as Alias } from ./relative

// Use only a subset.
use ./relative:namespace

// Use only a part of a subset.
use { Part } from ./relative:namespace

// Use only a part of a subset as a local alias.
use { Part as Alias } from ./relative:namespace
```

**3. Use internal project sources / packages. (absolute)**
```
// Use everything.
use @/src/local

// Use everything at path.
use @/src/local/deep/path

// Use everything as a local alias.
use @/src/local/path as Alias

// Use everything from package manifest.
use @/package

// Use only a part.
use { Part } from @/src/local/path

// Use only a part as a local alias.
use { Part as Alias } from @/src/local/path

// Use only a subset.
use @/src/local/path:namespace

// Use only a part of a subset.
use { Part } from @/src/local/path:namespace

// Use only a part of a subset as a local alias.
use { Part as Alias } from @/src/local/path:namespace
```

**4. Use external project sources / packages.**

> ℹ️ All non-standart packages are enforced to be scoped
```
// Use everything.
use @scope/package

// Use everything at path.
use @scope/package/deep/internals

// Use everything as a local alias.
use @scope/package as Alias

// Use only a part.
use { Part } from @scope/package

// Use only a part as a local alias.
use { Part as Alias } from @scope/package

// Use only a subset.
use @scope/package:namespace

// Use only a part of a subset.
use { Part } from @scope/package:namespace

// Use only a part of a subset as a local alias.
use { Part as Alias } from @scope/package:namespace
```

## Generic types, traits

```
TBD
```

## Namespaces

Namespaces are top-most building block and are used to group related building blocks together.

We can define & concatinate the namespaces as such

```
// src/user.x
ns User
ns User:Profile // ❌ Compilation error: Another encapsulating namespace block cannot be defined in the same file.

// Using a single `:` token
fn User:myFunction() {
    // Implementation
}

en User:UserRoles {
    USER
    ADMIN
}
```

### Example

Let's analyze this example.

```
// src/main.x

en UserRoles {  // ❌ Compilation Error: Another building block cannot be defined in the same file, unless it's part of an unique namespace.
    USER
    ADMIN
}

it User {  // ❌ Compilation Error: Another building block cannot be defined in the same file, unless it's part of an unique namespace.
    name: String
}

fn utilty() => { // ❌ Compilation Error: Another building block cannot be defined in the same file, unless it's part of an unique namespace.
    // Implementation exists
}

bp User(&) { // ❌ Compilation Error: Another building block cannot be defined in the same file, unless it's part of an unique namespace.
    // Implementation
}

fn main() => {
    say`Hello!`
}
```

To solve it, we extact the blocks into their corresponding files.

```
// src/util.x
fn utilty() => { // ✅ All good :)
    // Implementation exists
}
```

Then, we create a namespace to group all user related stuff in there.

```
// src/users.x
ns Users

en Users:UserRoles {
    USER
    ADMIN
}

bp Users:User(&) { // ✅ All good :)
    // Implementation
}

// src/main.x
import { Users } from `./users`

fn main() => {
    let user = Users:User()
    say`Hello, $user!`
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
fn example(p: ParamType) => ReturnType {
    // Function body
    // ...
    return returnValue
}

// Call
example({
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

#### Function arity

By default, every function receives one and only one object parameter and under the hood will expand this to individual arguments for the assembly interpretation where applicable.

But oftentimes there is need get more freedom in how a certain function is called.

```
// Declaration & Definition
@F::curry(3)
fn example(p1: T1, p2: T2, p3: T3) => {
  // Implementation
}

// Usage
example(1, 2, 3)
example(1, 2)(3)
example(1)(2, 3)
example(1)(2)(3)
```

#### Function constructor

The `fn` method is an alias for `F` blueprint primitive that is used for all functions.

```
// Log an empty pure function
log`$F`

// Create and call a pure function, returns void result
log`$F()`
```

#### Functional composability

The `F` constructor has many statically available functional methods that can be used natively in any form of loosely-coupled composability.

```
// Function declaration can be assigned a result of any functional operation.
fn getName = F::prop(`name`)

getName({
  name: `Jon`
})
```

Then more complex functional composability can be used.

```
// Using piping (Top to botton)
fn getMangledUserId = F::pipe(
  F::prop(`id`)
  F::prefix(`ID_`)
)

// Or using composability (Bottom to top)
fn getMangledUserId = F::compose(
  F::prefix(`ID_`)
  F::prop(`id`)
)

// Usage
getMangledUserId({
  id: `0000-0001`
})

// Using convergence
fn getMangledUserId = F::converge(F::concat)([
   F::constant(`___`)
   F::compose(
      F::prefix(`ID_`)
      F::prop(`id`)
   )
])

// Usage
getMangledUserId({
  id: `0000-0001`
})
```

## Decorators

Decorators are functional blueprints to vertically extend other building blocks in a non-destructive manner.

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

> ℹ️ Accessing a static blueprint method encapsulated inside a namespace looks like this:
>
> ```
> Namespace:Blueprint::staticMethod()
> ```

### Access modifiers

Access modifiers with zero runtime overhead, compile time memory alignment and performance optimizations.

By design there are 2 ways of looking at access modifiers.

#### Implicit

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

In vex there are two ways of coupling blueprints.

- **Strict** via blueprint parent inheritance
- **Loose** via blueprint dependency composition

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

## Enums

```
en Enumeration [
    ONE
    TWO
    THREE
]

// Usage
Enumeration.ONE
Enumeration.TWO
```

## Tables

```
tb Table [
  [1, 2, 3]
  [4, 5, 6]
  [7, 8, 9]
]
```

## Aliases

Aliases are a way to create zero-overhead alternative way on accessing a certain function, blueprint or field.

### For Functions

```

// Single alias
fn example() => {
  // Implementation
} as ex

// Many aliases
fn akwardlyLongFunctioName() => {
  // Implementation
} as [alfnx, alfn, af]

// 👇 All of theese point to one memory address
akwardlyLongFunctioName()
alfnx()
alfn()
af()

// Pointing to an existing alias anywhere in the context of your app/lib

// src/a.x
fn example() => {
  // Implementation
} as ex

// src/b.x
fn example() => {
  // Implementation
} as ex
// 👆 Compilation error: The alias `ex` already exists in [...]
```

### For Blueprints

> ℹ️ Blueprint aliases must start with a capital letter. Aliases are case-sensitive.

```
// Single alias
bp Example(&self) {
  // Implementation
} as Ex

// Many aliases
bp Example(&self) {
  // Implementation
} as [Exmp, Exm, Ex, EX] // 👈 Valid, as it's case sensitive
```

### For Blueprint fields

```
bp Example(&self) {
  // Single alias
  help: String = `Unique` as h

  // Many aliases
  group: String = `Unique` as [grp, gr, g]
}

// 👇 All the below are valid
Example().help
Example().h
Example().group
Example().grp
Example().gr
Example().g
```

### For package imports

```
import `@scope/name` as Alias
import { Something as Alias } from `@scope/name`

// Usage
Alias()
```
