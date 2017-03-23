## Variable Declarations

### `var` Declarations

`var` declarations have some odd scoping rules for those used to other languages. First, `var` declarations are accessible anywhere within their containing function, module, namespace, or global scope, regardless of the containing block. Some people call this `var`-scoping or *function-scoping*. Parameters are also function scoped.

```ts
function f(shouldInitialize: boolean) {
    if(shouldInitialize) {
        var x = 10;
    }

    return x;
}

f(true); // > 10
f(false): // > undefined
```

These scoping rules can cause several types of mistakes. One problem they exacerbate is the fact that it is not an error to declare the same variable multiple times.

### `let` Declarations

`var` has some problems, which is precisely why `let` statements were introduced.

```ts
let hello = "Hello!";
```

#### Block-Scoping

When a variable is declared is using `let`, it uses what some call *lexical-scoping* or *block-scoping*. Block-scoped variables are not visible outside of their nearest containing block or `for`-loop.

```ts
function f(input:boolean) {
    let a = 100;

    if(input) {
        // OK: to reference 'a'
        let b = a + 1;
        return b;
    }

    // ERR: 'b' doesn't exist here
    return b;
}
```

Another property of block-scoped variables is that they can't be read or written to before they're actually declared.

```ts
a++; // ERR: illegal to use 'a' before it's declared
let a;
```

> **NOTE:** You can still *capture* a block-scoped variable before it's declared. The only catch is that it's illegal to call that function before the declaration.

```ts
function foo() {
    // OK
    return a;
}

// ERR: illegal to call 'foo' before 'a' is declared
foo();

let a;
```

#### Re-Declarations and Shadowing

```ts
let x = 10;
let x = 20; // ERR: can't re-declare 'x' in the same scope
```

The act of introducing a new name in a more nested scope is called *shadowing*. It is a bit of a double-edge sword in that it can introduce certain bugs on its own in the event of accidental shadowing, while also preventing certain bugs. Shadowing should *usually* be avoided in the interest of writing cleaner code.

#### Block-Scoped Variable Capturing

`let` declarations have drastically different behavior when declared as part of a loop. Rather than just introducing a new environment to the loop itself, these declarations sort of create a new scope *per iteration*.

```ts
for (let i = 0; i < 10; i++) {
    setTimeout(function() { console.log(i); }, 100 * i);
}
```

### `const` Declarations

`const` variables are like `let` declarations but, as their name implies, their value cannot be changed once they are bound.

```ts
const numLivesForCat = 9;
```

This should not be confused with the idea that the values they refer to are *immutable*.

```ts
const numLivesForCat = 9;
const kitty = {
    name: "Aurora",
    numLives: numLivesForCat,
}

// ERR
kitty = {
    name: "Danielle",
    numLives: numLivesForCat,
};

// OK
kitty.name = "Rory";
kitty.name = "Kitty";
kitty.name = "Cat";
kitty.numLives--;
```

Unless you take specific measures to avoid it, the internal state of a `const` variable is still modifiable. Fortunately, TypeScript allows you to specify that members of an object are `readonly`.

### `let` vs. `const`

Applying the *principle of least privilege*, all declarations other than those you plan to modify should use `const`. The rationale is that if a variable didn't need to get written to, others working on the same codebase shouldn't automatically be able to write to the object, and will need to consider whether they really need to reassign to the variable. Using `const` also makes the code more predictable when reasoning about flow of data.

### Destructuring

#### Array Destructuring

```ts
let input = [1, 2];
let [first, second] = input;
console.log(first); // > 1
console.log(second); // > 2

let [first] = [1, 2, 3, 4];
let [, second, , fourth] = [1, 2, 3, 4];

// swap variables
[first, second] = [second, first];

function f([first, second]: [number, number]) {
    console.log(first);
    console.log(second);
}
f([1, 2]);
```

You can create a variable for the remaining items in a list using the syntax `...`:

```ts
let [first, ...rest] = [1, 2, 3, 4];
console.log(first); // > 1
console.log(rest); // > [ 2, 3, 4 ]
```

#### Object Destructuring

```ts
let o = {
    a: "foo",
    b: 12,
    c: "bar"
}

let { a, b } = o;

(
    { a, b } =
        {
            a: "baz",
            b: 101
        }
);
```

You can create a variable for the remaining items in an object using the syntax `...`:

```ts
let { a, ...passthrough } = o;
let total = passthrough.b + passthrough.c.length;
```

You can also give different names to the properties:

```ts
let { a: newName1, b: newName2 } = o;
let { a, b }: { a: string, b: number } = o;
```

If a property is undefined, you can specify a default value.

```ts
function keepWholeObject(wholeObject: { a: string, b?: number }) {
    let { a, b = 1001 } = wholeObject;
}
```

#### Function Declarations

```ts
type C = { a: string, b?: number }

function f({ a, b }: C): void {
    // ...
}

// 1. Put type before default value.
function f({ a, b } = { a: "", b: 0 }): void {
    // ...
}
f(); // OK: defaults to { a: "", b: 0 }

// 2. Give a default for optional properties
function f({ a, b = 0 } = { a: "" }) : void {
    // ...
}
f({ a: "yes" }); // OK: defaults to b = 0
f(); // OK: defaults to { a: "" }, which then defaults to b = 0
f({ }); // ERR: 'a' is required if you supply an argument
```

#### Spread

The spread operator is opposite of destructuring. It allows you to spread an array into another array, or an object into another object.

```ts
let first = [1, 2];
let second = [3, 4];

let bothPlus = [0, ...first, ...second, 5];
```

Spreading creates a shallow copy of `first` and `second`. They are not changed by the spread.

You can also spread objects:

```ts
let defaults = {
    food: "spicy",
    price: "$$",
    ambiance: "noisy"
};

let search = {
    ...defaults,
    food: "rich"
};
```

Object spread has a couple of limitations. First, it only includes own, enumerable properties. Basically, that means you lose methods when you spread.

```ts
class C {
    pm = 12;
    m() {
    }
}

let c = new C();
let clone = { ...c };
clone.p; // OK
clone.m(); // ERR!
```

Second, the TypeScript compiler doesn't allow spreads of type parameters from generic functions. That feature is expected in future versions of the language.
