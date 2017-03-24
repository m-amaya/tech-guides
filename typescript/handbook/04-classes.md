## Classes

```ts
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello," + this.greeting;
    }
}

let greeter = new Greeter("world");
```

### Inheritance

```ts
class Animal {
    name: string;
    constructor(theName: string) {
        this.name = theName;
    }
    move(distanceInMeters: number = 0) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}

// derived classes that contain constructor functions must call `super()`
// which will execute the constructor function on the base class
class Snake extends Animal {
    constructor(name: string) {
        super(name);
    }

    // overrides base class method
    move(distanceInMeters = 5) {
        console.log("Slithering...");
        super.move(distanceInMeters);
    }
}

class Horse extends Animal {
    constructor(name: string) {
        super(name);
    }

    // overrides base class method
    move(distanceInMeters = 45) {
        console.log("Galloping...");
        super.move(distanceInMeters);
    }
}

let sam = new Snake("Sammy the Python");
let tom: Animal = new Horse("Tommy the Palomino");

sam.move();
tom.move(34);
```

### Public, Private, and Protected Modifiers

In TypeScript, each class member is `public` by default. You may still mark a member `public` explicitly.

```ts
class Animal {
    public name: string;
    public constructor(theName: string) {
        this.name = theName;
    }
    public move(distanceInMeters: number) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}
```

#### Understanding `private`

When a member is marked as `private`, it cannot be accessed from outside of its containing class.

```ts
class Animal {
    private name: string;
    constructor(theName: string) {
        this.name = theName;
    }
}

new Animal("Cat").name; // ERR! 'name' is private;
```

For two types to be considered compatible, if one of them has a `private` member, than the other must have a `private` member that originated in the same declaration. The same applies to `protected` members.

```ts
class Animal {
    private name: string;
    constructor(theName: string) { this.name = theName; }
}

class Rhino extends Animal {
    constructor() { super("Rhino"); }
}

class Employee {
    private name: string;
    constructor(theName: string) { this.name = theName; }
}

let animal = new Animal("Goat");
let rhino = new Rhino();
let employee = new Employee("Bob");

animal = rhino;
animal = employee; // ERR! 'Animal' and 'Employee' are not compatible
```

#### Understanding `protected`

The `protected` modifier acts much like that `private` modifier with the exception that members declared `protected` can also be accessed by instances of deriving classes.

```ts
class Person {
    protected name: string;
    constructor(name: string) { this.name = name; }
}

class Employee extends Person {
    private department: string;

    constructor(name: string, department: string) {
        super(name);
        this.department = department;
    }

    public getElevatorPitch() {
        return `Hello, my name is ${this.name} and I work in ${this.department}.`;
    }
}

let howard = new Employee("Howard", "Sales");
console.log(howard.getElevatorPitch());
console.log(howard.name); // ERR!
```

A constructor may also be marked `protected`. This means that the class cannot be instantiated outside of its containing class, but can be extended.

```ts
class Person {
    protected name: string;
    protected constructor(theName: string) { this.name = theName; }
}

// Employee can extend Person
class Employee extends Person {
    private department: string;

    constructor(name: string, department: string) {
        super(name);
        this.department = department;
    }

    public getElevatorPitch() {
        return `Hello, my name is ${this.name} and I work in ${this.department}.`;
    }
}

let howard = new Employee("Howard", "Sales");
let john = new Person("John"); // ERR! The 'Person' constructor is protected
```

### Readonly Modifier

```ts
class Octopus {
    readonly name: string;
    readonly numberOfLegs: number = 8;
    constructor (theName: string) {
        this.name = theName;
    }
}

let dad = new Octopus("Man with the 8 strong legs");
dad.name = "Man with the 3-piece suit"; // ERR! 'name' is 'readonly'
```

#### Parameter Properties

*Parameter properties* let you create and initialize a member in one place. This works for `readonly`, `private`, `public`, and `protected` keywords.

```ts
class Octupus {
    readonly numberOfLegs: number = 8;
    constructor(readonly name: string) {
    }
}
```

### Accessors

```ts
let passcode = "secret passcode";

class Employee {
    private _fullName: string;

    get fullName(): string {
        return this._fullName;
    }

    set fullName(newName: string) {
        if (passcode && passcode === "secret passcode") {
            this._fullName = newName;
        } else {
            console.log("ERR! Unauthorized update of employee!");
        }
    }
}

let employee = new Employee();
employee.fullName = "Bob Smith";
if (employee.fullName) {
    console.log(employee.fullName);
}
```

> **NOTE:** Accessors require you to set the compiler to output ESCMAScript 5 or higher.

> **NOTE:** Accessors with a `get` and `set` are automatically inferred to be `readonly`.

### Static Properties

Up to this point, we've only talked about the **instance** members of the class. We can also create **static** members, those that are visible on the class itself rather than on the instances.

```ts
class Grid {
    static origin = { x: 0, y: 0 };
    calculateDistanceFromOrigin(point: { x: number; y: number; }) {
        let xDist = (point.x - Grid.origin.x);
        let yDist = (point.y - Grid.origin.y);
        return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale;
    }
    constructor(public scale: number) { }
}

let grid1 = new Grid(1.0); //> 1x scale
let grid2 = new Grid(5.0); //> 5x scale

console.log(grid1.calculateDistanceFromOrigin({ x: 10, y: 10 }));
console.log(grid2.calculateDistanceFromOrigin({ x: 10, y: 10 }));
```

### Abstract Classes

**Abstract classes** are base classes from which other classes may be derived. They may not be instantiated directly. Unlike an interface, an abstract class may contain implementation details for its members.

```ts
abstract class Animal {
    abstract makeSound(): void;
    move(): void {
        console.log("roaming the earth...");
    }
}
```

```ts
abstract class Department {
    constructor(public name: string) { }

    printName(): void {
        console.log("Department name: " + this.name);
    }

    abstract printMeeting(): void; //> must be implemented in derived classes
}

class AccountingDepartment extends Department {
    constructor() {
        super("Accounting and Auditing"); //> constructors in derived classes must call super()
    }

    printMeeting(): void {
        console.log("The Accounting Department meets each Monday at 10am.");
    }

    generateReports(): void {
        console.log("Generating accounting reports...");
    }
}

let department: Department; // OK, creates a reference to an abstract type
department = new Department(); // ERR! cannot create an instance of an abstract class
department = new AccountingDepartment(); // OK, creates and assigns a non-abstract sub-class
department.printName();
department.printMeeting();
department.generateReports(); // ERR! method doesn't exist on declared abstract type
```

### Advanced Techniques

#### Constructor Functions

```ts
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}

let greeter: Greeter;
greeter = new Greeter("world");
console.log(greeter.greet());
```

#### Using a Class as an Interface

```ts
class Point {
    x: number;
    y: number;
}

interface Point3d extends Point {
    z: number;
}

let point3d: Point3d = { x: 1, y: 2, z: 3 };
```
