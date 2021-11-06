# Everyday Types


### The primitives: `string`, `number` and `boolean`
---

1. `const message: string = "Hello world"`
2. `const val: number = 42`
3. `const isLogin: boolean = true`

### Arrays
---

To specify the type of an array like [1, 2, 3], you can use the syntax number[]; this syntax works for any type (e.g. string[] is an array of strings, and so on). You may also see this written as Array<number>, which means the same thing. We’ll learn more about the syntax `T<U>` when we cover generics.
    
    eg. const arr: number[] = [1,2,3]

### `any`

TypeScript also has a special type, any, that you can use whenever you don’t want a particular value to cause typechecking errors.

When a value is of type any, you can access any properties of it (which will in turn be of type any), call it like a function, assign it to (or from) a value of any type, or pretty much anything else that’s syntactically legal.

### `noImplicitAny`

When you don’t specify a type, and TypeScript can’t infer it from context, the compiler will typically default to any.

You usually want to avoid this, though, because any isn’t type-checked. Use the compiler flag noImplicitAny to flag any implicit any as an error.

---

> When you declare a variable using const, var, or let, you can optionally add a type annotation to explicitly specify the type of the variable.
> In most cases, though, this isn’t needed. Wherever possible, TypeScript tries to automatically infer the types in your code. For example, the type of a variable is inferred based on the type of its initializer.
> For the most part you don’t need to explicitly learn the rules of inference. If you’re starting out, try using fewer type annotations than you think - you might be surprised how few you need for TypeScript to fully understand what’s going on.  



### Functions 
---  

1. Can declare types of parameters inside a function. Paramter type annotations go after the parameter name.

```
//Code Example
function greet(name: string) {
    console.log("Hello, " + name.toUpperCase() + "!!");
}
```
2. When a parameter has a type annotation arguments to that function will be checked

3. Even if you don’t have type annotations on your parameters, TypeScript will still check that you passed the right number of arguments.  

4. Can also add annotations to the return type. 

    ```
    function getNumber(): number {
        return 26;
    }
    ```

5. Much like variable type annotations, you usually don’t need a return type annotation because TypeScript will infer the function’s return type based on its return statements.

6. In case of Anonymous functions, Typescript automatically determines how and in what context it is being called, then the parameters of that function are automatically given types.

        //for eg.
        // No type annotations here, but TypeScript can spot the bug
        const names = ["Alice", "Bob", "Eve"];
        
        // Contextual typing for function
        names.forEach(function (s) {
            console.log(s.toUppercase());
        //Property 'toUppercase' does not exist on type 'string'. Did you mean 'toUpperCase'?
        });
        
        // Contextual typing also applies to arrow functions
        names.forEach((s) => {
            console.log(s.toUppercase());
        //Property 'toUppercase' does not exist on type 'string'. Did you mean 'toUpperCase'?
        });

### Objects
---

* If you don't specify a type to the property of object, it will be assumed to be `any`.

* Also you can specify that some or all of their properties are optional. To do this, add a `?` after the property name.

### Union Types
---

* TypeScript allows you to build new types out of existing ones using a large variety of operators.

* A union type is a type formed from two or more other types, representing values that may be any one of those types. We refer to each of these types as the union's member.

```
function printId(id: number | string) {
  console.log("Your ID is: " + id);
}
// Error
printId({ myID: 22342 });
Argument of type '{ myID: number; }' is not assignable to parameter of type 'string | number'.
Type '{ myID: number; }' is not assignable to type 'number'.
```
TypeScript will only allow you to do things with the union if that thing is valid for every member of the union. For example, if you have the union `string | number`, you can’t use methods that are only available on `string`.

### Type Aliases
---  

        type Point = {
            x: number;
            y: number;
        };
 
        // Exactly the same as the earlier example
        function printCoord(pt: Point) {
            console.log("The coordinate's x value is " + pt.x);
            console.log("The coordinate's y value is " + pt.y);
        }
 
        printCoord({ x: 100, y: 100 });

### Interfaces (Another way to name an object type)
---

        interface Point {
            x: number;
            y: number;
        }
 
        function printCoord(pt: Point) {
            console.log("The coordinate's x value is " + pt.x);
            console.log("The coordinate's y value is " + pt.y);
        }
 
        printCoord({ x: 100, y: 100 });

### Difference b/w Type Aliases and Interfaces
---

* Key distinction is that a type cannot be re-opened to add new properties vs an interface which is always extendable.

| Interface | Type |
|---------- | --------|
| **Extending an Interface** | **Extending a type via intersections** |


### Literal Types
---

* Literal types aren't very valuable but by combining literals into unions, you can express a much more useful concept - for example, functions that only accept a certain set of known values

* For eg. 
```
function printText(s: string, alignment: "left" | "right" | "center") {
  // ...
}
printText("Hello, world", "left");
printText("G'day, mate", "centre");
//Argument of type '"centre"' is not assignable to parameter of type //'"left" | "right" | "center"'.
```

### Literal Inference
---

* When you initialize a variable with an object, TypeScript assumes that the properties of that object might change values later. For example, if you wrote code like this

```
const obj = { counter: 0 };
if (someCondition) {
  obj.counter = 1;
}
```
* TypeScript doesn’t assume the assignment of 1 to a field which previously had 0 is an error. Another way of saying this is that obj.counter must have the type number, not 0, because types are used to determine both reading and writing behavior

* Same Applies to Strings for eg.

```
const req = { url: "https://example.com", method: "GET" };
handleRequest(req.url, req.method);
//Argument of type 'string' is not assignable to parameter of type '"GET" | "POST"'.
```
* In the above example req.method is inferred to be string, not "GET". Because code can be evaluated between the creation of req and the call of handleRequest which could assign a new string like "GUESS" to req.method, TypeScript considers this code to have an error

* Solution: 

```
// Change 1:
const req = { url: "https://example.com", method: "GET" as "GET" };
// Change 2
handleRequest(req.url, req.method as "GET");
// Change 3
const req = { url: "https://example.com", method: "GET" } as const;
handleRequest(req.url, req.method);
```
* Change 1 means “I intend for req.method to always have the literal type "GET"”, preventing the possible assignment of "GUESS" to that field after. Change 2 means “I know for other reasons that req.method has the value "GET"“.

* The as const suffix acts like const but for the type system, ensuring that all properties are assigned the literal type instead of a more general version like string or number

### `strictNullChecks` off
---

* With strictNullChecks off, values that might be `null` or `undefined` can still be accessed normally, and the values `null` and `undefined` can be assigned to a property of any type. This is similar to how languages without null checks (e.g. C#, Java) behave. The lack of checking for these values tends to be a major source of bugs; we always recommend people turn strictNullChecks on if it’s practical to do so in their codebase.

### `strictNullChecks` on
---
With strictNullChecks on, when a value is `null` or `undefined`, you will need to test for those values before using methods or properties on that value. Just like checking for `undefined` before using an optional property, we can use narrowing to check for values that might be `null`

### Less Common Primitives
---

* New primitive Introduced in JS since ES2020. Used for storing very large Integers. `BigInt`

```
// Creating a bigint via the BigInt function
const oneHundred: bigint = BigInt(100);
 
// Creating a BigInt via the literal syntax
const anotherHundred: bigint = 100n;
```
* `symbol` - This primitive is used to create a globally unique reference via the function `Symbol()`:

```
const firstName = Symbol("name"); // creates new reference 
const secondName = Symbol("name"); // creates another new reference
 
if (firstName === secondName) {
//This condition will always return 'false' since the types 'typeof //firstName' and 'typeof secondName' have no overlap.
  // Can't ever happen
}
```



