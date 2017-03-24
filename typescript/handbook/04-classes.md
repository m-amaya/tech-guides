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

```

### Readonly Modifier

### Accessors

### Static Properties

### Abstract Classes

### Advanced Techniques
