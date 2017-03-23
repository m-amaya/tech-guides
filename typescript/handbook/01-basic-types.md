## Basic Types

### Boolean

```ts
let isDone: boolean = false;
```

### Number

```ts
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```

### String

```ts
let color: string = "blue";
color = 'red';

let fullName: string = `Bob Bobbington`;
let age: number = 37;
let sentence: string = `Hello, my name is ${ fullName }.

I'll be ${ age + 1 } years old next month.`
```

### Array

```ts
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];
```

### Tuple

```ts
let x: [string, number];
x = ["hello", 10];

console.log(x[0].substr(1));

x[3] = "world"; // OK, 'string' can be assigned to 'string | number'
x[5].toString(); // OK, 'string' and 'number' both have 'toString'
x[6] = true; // ERR, 'boolean' isn't 'string | number'
```

### Enum

```ts
enum Color { Red, Green, Blue };
let c: Color = Color.Green;

enum Color { Red = 1, Green = 2, Blue = 4 };
let c:Color = Color.Green;
let colorName: string = Color[2];
console.log(colorName) // 'Green'

```

### Any

```ts
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // OK, definitely a Boolean

notSure.ifItExists(); // OK, ifItExists might exist at runtime
notSure.toFixed(); // OK, toFixed exists (but the compiler doesn't check)

let prettySure: Object = 4;
prettySure.toFixed(); // ERR, property 'toFixed' doesn't exist on type 'Object'

let list: any[] = [ 1, true, "free" ];

list[1] = 100;
```

### Void

`void` is the absence of having any type at all. You may commonly see this as the return type of functions that do not return a value:

```ts
function warnUser(): void {
    alert("This is my warning message");
}
```

Declaring variables of type `void` is not useful because you can only assign `undefined` or `null` to them;

```ts
let unusable: void = undefined;
```

### Null and Undefined

```ts
let u: undefined = undefined;
let n: null = null;
```

### Never

Function returning `never` must have unreachable end point

```ts
function error(message: string): never {
    throw new Error(message);
}

function infiniteLoop(): never {
    while(true) {
    }
}
```

Inferred return type is `never`.

```ts
function fail() {
    return error("Something failed");
}
```

### Type Assertions

**Type assertions** are a way to tell the compiler "trust me, I know what I'm doing". A type assertion is like a type cast in other languages, but performs no special checking or restructuring of data. It has no runtime impact, and is used purely by the compiler. TypeScript assumes that you, the programmer, have performed any special checks that you need.

```ts
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
let strLength: number = (someValue as string).length;
```

> **Note:** When using TypeScript with JSX, only `as`-style assertions are allowed.

### A Note About `let`

Many common problems in JavaScript are alleviated by using `let`, so you should use it instead of `var` whenever possible.
