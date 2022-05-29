# Typescript

TypeScript checks a program for errors before execution, and does so based on the kinds of values, it’s a static type checker. TypeScript is a language that is a superset of JavaScript: JS syntax is therefore legal TS. 
However, TypeScript is a typed superset, meaning that it adds rules about how different kinds of values can be used.

Typescript's extension is .ts and when its code is compiled, it produces a .js file.

```typescript
function greeter(person) {
  return "Hello, " + person;
}
let user = "Jane User";
document.body.textContent = greeter(user);
```

Run below command for typescript compiler.
```bash
tsc greeter.ts
```

On running above command , a file called greeter.js is produced. Now, add a : string type annotation to the ‘person’ function argument as shown below:

```typescript
function greeter(person: string) {
  return "Hello, " + person;
}
let user = "Jane User";
document.body.textContent = greeter(user);
```
Similarly, if we use an array in user variable above code will run but it will give a compile time error, though on compiling it would still generate a .js file.

## Interfaces

Interface is like a struct which has some predefined structure. For example, if there is an interface called Person , we can define it with two properties such as firstname and lastname. Two interfaces are compatible if their internal structures are compatible.

```typescript
interface Person {
  firstName: string;
  lastName: string;
}
function greeter(person: Person) {
  return "Hello, " + person.firstName + " " + person.lastName;
}
let user = { firstName: "Jane", lastName: "User" };
document.body.textContent = greeter(user);
```

## Classes 

Class in typescript has both fields and constructor as well. For example, see below:

```typescript
class Student {
  fullName: string;
  constructor(
    public firstName: string,
    public middleInitial: string,
    public lastName: string
  ) {
    this.fullName = firstName + " " + middleInitial + " " + lastName;
  }
}
interface Person {
  firstName: string;
  lastName: string;
}
function greeter(person: Person) {
  return "Hello, " + person.firstName + " " + person.lastName;
}
let user = new Student("Jane", "M.", "User");
document.body.textContent = greeter(user);
```
Every value in typescript has set of behaviours , we can observe by running different operations.

```typescript
let message = "Hello World"
message.toLowerCase();
console.log(message.toLowerCase());
```




