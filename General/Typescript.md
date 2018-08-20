**Typescript**

Typescript is a super-set of the javascript language. It ads some nice features to the language that makes the code less error prone. We can assign types to variables or properties. This stops us from creating things of the wrong type.

We also get some ES6 features like:
- class creation with the class keyword
- using interfaces to create contracts out of classes that variables have to fulfil
- using generic typescript
- using modules

*Using Types*

`let myString: string;`

`myString` is of type string and cannot be compiled if it is assigned to anything other than a string a string.

`let anotherString = 'This is a string without : string'`

Typescript is also able to infer types. Typescript would infer that `anotherString` is a string. You do not need to use explicit typing when assigning a value at the declaration time.

```
let yetAnotherString;

yetAnotherString = 4;

yetAnotherString = 'This still works!'
```

You don't have to set a type when you declare a variable. The above would not throw an error, but it is best practice to declare types. It is unable to infer if type isn't specifically declared or the value isn't assigned at declaration time.

Basic typescript types are:

```
let aString: string;
let aNumber: number;
let aBoolean: boolean;
let anArray: Array<string>; // This is a generic type. the example can only hold strings.
let anything: any;
// We also have void for functions which means they do not return a value and enums. Our own created classes can be assigned as types.
```

**Classes**
Typescript automatically compiles down into old javascript so it can be used on a multitude of browsers.

```
class Car {
  engineName: string;
  gears: number;
  private speed: number; //The private keyword means it's only accessible from within this class.

  constructor(speed: number) {
    this.speed = speed || 0;
  }

  accelerate(): void{
    this.speed ++;
  }

  throttle(): void {
    this.speed --;
  }

  getSpeed(): void {
    console.log(this.speed);
  }

  static numberOfWheels(): number {
    return 4
  }
  //Static methods are methods that exist on the class rather than an instance on the class.
}
```

**Interfaces**

An interface is like a contract saying whatever implements this interface must have the properties. Interfaces allow us to create a pure form of communication between several objects. This allows us to tell other objects they can use the interface (e.g onInit in angular). It also lets us create pseudo classes that we can use as types.


```
interface User {
  username: string;
  password: string;
  confirmPassword?: // string; not necessary as ? is there
}

let user: User

user = {anything: 'max', anynumber: 5}; //does not work
user = {username: 'max', password: 'partytime'}; //does work
```

Interfaces can also be functions:

```
interface CanDrive = {
  accelerate: function (speed:number): void;
}
// Whatever implements can drive should have this method.

let car:CanDrive = {
  accelerate: function (speed: number) {
    //...
  }
}
```

**Generics**

Generics allow us to be flexible with the types object use.

```
let numberArray: Array<number>; //this is an array of numbers

numberArray = ['test']; // would not work
numberArray - [1,2,3]; // would work
```

**Modules**

We can tell typescript the things we want available from outside the file. Modularity is about splitting our code up through several files.

```
export class ExportedClass {

}
```
