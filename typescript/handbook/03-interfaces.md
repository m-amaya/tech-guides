## Interfaces

One of TypeScript's core principles is that type-checking focuses on the *shape* that the values have. This is sometimes called **duck typing** or **structural subtyping**. In TypeScript, interfaces fill the role of naming these types, and are a powerful way of defining contracts within your code as well as contracts with code outside of your project.

### Our First Interface

```ts
interface LabeledValue {
    label: string;
}

function printLabel(labelObj: LabeledValue) {
    console.log(labelObj.label);
}

let myObj = {
    size: 10,
    label: "Size 10 Object"
}

printLabel(myObj);
```

### Optional Properties

```ts
interface SquareConfig {
    color?: string;
    width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
    let newSquare = { color: "white", area: 100 };

    if(config.color) {
        newSquare.color = config.color;
    }

    if(config.width) {
        newSquare.area = config.width * config.width;
    }

    return newSquare;
}

let mySquare = createSquare({ color: "black" });
```

### Readonly Properties

```ts
interface Point {
    readonly x: number;
    readonly y: number;
}

let p1: Point = { x: 10, y: 20 };
p1.x = 5; // ERR!
```

```ts
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; // ERR!
ro.push(5); // ERR!
ro.length = 100; // ERR!

a = ro; // ERR!
// override with a type assertion
a = ro as number[];
```

#### `readonly` vs. `const`

The easiest way to remember whether to use readonly or const is to ask whether you're using it on a variable or a property. Variables use `const` whereas properties use `readonly`.

### Excess Property Checks

If an object literal has any properties that the "target type" doesn't have, you'll get an error.

```ts
interface SquareConfig {
    color?: string;
    width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
    // ...
}

// ERR! 'colour' not expected in type 'SquareConfig'
let mySquare = createSquare({ colour: "red", width: 100 });
```

You can get around these checks by using type assertion.

```ts
let mySquare = createSquare({ width: 100, opacity: 0.5 } as SquareConfig);
```

However, a better approach might be to add a string index signature if you're sure that the object can have some extra properties that are used in some special way.

```ts
interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
}
```

One final way to get around these checks is to assign the object to another variable.

```ts
let squareOptions = { colour: "red", width: 100 };
let mySquare = createSquare(squareOptions);
```

### Function Types

```ts
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    let result = source.search(subString);
    return result > -1;
}

// -or-

let mySearch: SearchFunc;
mySearch = function (src: string, sub: string): boolean {
    let result = src.search(sub);
    return result > - 1;
}

// -or-

let mySearch: SearchFunc;
mySearch = function (src, sub) {
    let result = src.search(sub);
    return result > -1;
}
```

### Indexable Types

Indexable types have an *index signature* that describes the type we can use to index into the object, along with the corresponding return types when indexing.

```ts
// indicates that when a `StringArray` is indexed with a `number`,
// it will return a `string`
interface StringArray {
    [index: number]: string;
}

let myArray: StringArray;
myArray = [ "Bob", "Fred" ];

let myStr: string = myArray[0];
```

There are two types of supported index signatures: `string` and `number`. It is possible to support both types of indexers, but the type returned from a numeric indexer must be a subtype of the type returned from the string indexer. This is because when indexing with a `number`, JavaScript will actually convert that to a `string` before indexing into an object.

```ts
class Animal {
    name: string;
}

class Dog extends Animal {
    breed: string;
}

// ERR! Indexing with a 'string' will sometimes get you a Dog!
interface NotOkay {
    [x: number]: Animal;
    [x: string]: Dog;
}
```

```ts
interface NumberDictionary {
    [index: string]: number;
    length: number; // OK, length is a number
    name: string; // ERR! the type of 'name' is not a subtype of the indexer
}
```

You can make index signatures `readonly` in order to prevent assignment to their indices.

```ts
interface ReadonlyStringArray {
    readonly [index: string]: string;
}

let myArray: ReadonlyStringArray = [ "Alice", "Bob" ];
myArray[2] = "Mallory"; // ERR!
```

### Class Types

```ts
interface ClockInterface {
    currentTime: Date;
}

class Clock implements ClockInterface {
    currentTime: Date;
    constructor(h: number, m: number) { }
}
```

You can also describe methods in an interface that are implemented in the class.

```ts
interface ClockInterface {
    currentTime: Date;
    setTime(d: Date);
}

class Clock implements ClockInterface {
    currentTime: Date;
    setTime(d: Date) {
        this.currentTime = d;
    }
    constructor(h: number, m: number) { }
}
```

Interfaces describe the public side of the class, rather than both the public and private side.

#### Difference between the static and instance sides of classes

A class has 2 types:

1. the type of the static side
2. the type of the instance side

```ts
// the constructor
interface ClockConstructor {
    new (hour: number, minute: number): ClockInterface;
}

// the instance methods
interface ClockInterface {
    tick();
}

// creates instances
function createClock(constructor: ClockConstructor, hour: number, minute: number): ClockInterface {
    return new constructor(hour, minute);
}

class DigitalClock implements ClockInterface {
    constructor(h: number, m: number) { }
    tick() {
        console.log("beep beep");
    }
}

class AnalogClock implements ClockInterface {
    constructor(h: number, m: number) { }
    tick() {
        console.log("tick tock");
    }
}

let digital = createClock(DigitalClock, 12, 17);
let analog = createClock(AnalogClock, 7, 32);
```

### Extending Interfaces

```ts
interface Shape {
    color: String;
}

interface Square extends Shape {
    sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
```

An interface can extend multiple interfaces, creating a combination of all of the interfaces.

```ts
interface Shape {
    color: string;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```

### Hybrid Types

```ts
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void
}

function getCounter(): Counter {
    let counter = <Counter>function (start: number) { };
    counter.interval = 123;
    counter.reset = function() { };
    return counter;
}

let c = getCounter();
c(10);
c.reset();
c.interval = 5.0;
```

When interacting with 3rd-party JavaScript, you may need to use patterns like the above to fully describe the shape of the type.

### Interface Extending Classes

```ts
class Control {
    private state: any;
}

interface SelectableControl extends Control {
    select(): void;
}

class Button extends Control {
    select() { }
}

class TextBox extends Control {
    select() { }
}

class Image {
    select() { }
}

class Location {
    select() { }
}
```
