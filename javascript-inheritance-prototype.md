
# JavaScript Inheritance Cheat Sheet

This cheat sheet provides an overview of inheritance in JavaScript using three different approaches: objects, constructor functions, and ES6 classes. It explains how inheritance works and provides examples for each approach.

## 1. Object-based Inheritance

In JavaScript, you can create an inheritance relationship by manipulating the `prototype` object.

### Example:
```js
const person = {
  name: 'John',
  greet() {
    return `Hi, I'm ${this.name}`;
  }
};

// Create an instance using Object.create to inherit from person
const alice = Object.create(person);
alice.name = 'Alice';

console.log(alice.greet());  // Output: "Hi, I'm Alice"
```

### Key Points:
- You can use `Object.create` to set up inheritance.
- The object created this way shares the properties and methods of the prototype object.

## 2. Constructor Function-based Inheritance

Before ES6, JavaScript used constructor functions to set up inheritance. We use `Person.prototype` to share methods between all instances created by the `Person` constructor function.

### Example:
```js
function Person(name) {
  this.name = name;
}

// Add method to the prototype
Person.prototype.greet = function() {
  return `Hi, I'm ${this.name}`;
};

const alice = new Person('Alice');
console.log(alice.greet());  // Output: "Hi, I'm Alice"
```

### Key Points:
- The constructor function is used to initialize instances.
- Methods are defined on `Person.prototype`, which all instances of `Person` inherit.
- You can set the constructor property of the prototype manually when changing inheritance.

### Inheriting from another Constructor:
```js
function Employee(name, position) {
  Person.call(this, name);  // Inherit from Person
  this.position = position;
}

// Inherit methods from Person
Employee.prototype = Object.create(Person.prototype);
Employee.prototype.constructor = Employee;

const emp = new Employee('Alice', 'Developer');
console.log(emp.greet());  // Output: "Hi, I'm Alice"
```

### Key Points:
- `Object.create(Person.prototype)` is used to establish the inheritance chain.
- The `constructor` property is reset to the new constructor (`Employee`).

## 3. Class-based Inheritance (ES6 Classes)

ES6 introduced a more structured way to handle inheritance with the `class` syntax. It simplifies the process of setting up inheritance and constructor initialization.

### Example:
```js
class Person {
  constructor(name) {
    this.name = name;
  }

  greet() {
    return `Hi, I'm ${this.name}`;
  }
}

class Employee extends Person {
  constructor(name, position) {
    super(name);  // Call the parent class's constructor
    this.position = position;
  }
}

const emp = new Employee('Alice', 'Developer');
console.log(emp.greet());  // Output: "Hi, I'm Alice"
```

### Key Points:
- The `class` keyword is used to define the constructor and methods.
- `extends` is used to establish inheritance.
- `super()` is used to call the parent class constructor from the child class.

### Summary:
- **Object-based inheritance** is the most basic approach using `Object.create`.
- **Constructor function-based inheritance** uses the `prototype` object and `call()`/`apply()` to invoke parent constructors.
- **Class-based inheritance** provides a more intuitive and structured approach with `extends` and `super()`.

---

## Prototypes and Inheritance
- In JavaScript, every function has a `prototype` property, which is an object that holds methods and properties shared by all instances of that function.
- Inheritance happens by linking one object's prototype to another. When you try to access a property or method, JavaScript looks up the prototype chain until it finds the property.

## Conclusion

This cheat sheet covers inheritance in JavaScript with three different approaches. ES6 class-based inheritance is recommended for modern JavaScript due to its simplicity and readability.
