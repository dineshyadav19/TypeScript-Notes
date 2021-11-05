## TypeScript Notes
---

* JavaScript only truly provides dynamic typic typing - running the code to see what happens.

* For some values, such as the primitives ```string``` and ```number```, we can identify their type at runtime using the ```typeof``` operator. But for other things like functions, there’s no corresponding runtime mechanism to identify their types.

* **Alternative** - use a static type system to make predictions about what code is expected before it runs.

* Static types systems describe the shapes and behaviors of what our values will be when we run our programs. A type-checker like **TypeScript** uses that information and tells us when things might be going off the rails.

* In case of objects, If we try to access a property that doesn't exist should throw an error but JS returns the value `undefined`.

```const user = {
  name: "Daniel",
  age: 26,
  };
  user.location; // returns undefined
```
* But in case of TypeScript this would throw an error about location being not defined .

### Types for Tooling
---
* TypeScript can be used while editing code as well.
* The type-checker has information to check things like whether we’re accessing the right properties on variables and other properties. Once it has that information, it can also start suggesting which properties you might want to use.

* `tsc filename.ts`  command to run the ts file.
* This command creates another file with .js extension along with the .ts file.

### Explicit Types
---
* Add type annotations to tell typecript which is what.
* we don’t always have to write explicit type annotations. In many cases, TypeScript can even just infer (or “figure out”) the types for us even if we omit them.
* Even though we didn’t tell TypeScript that msg had the type string it was able to figure that out. That’s a feature, and it’s best not to add annotations when the type system would end up inferring the same type anyway.

**Type annotations aren’t part of JavaScript (or ECMAScript to be pedantic), so there really aren’t any browsers or other runtimes that can just run TypeScript unmodified. That’s why TypeScript needs a compiler in the first place - it needs some way to strip out or transform any TypeScript-specific code so that you can run it. Most TypeScript-specific code gets erased away, and likewise, here our type annotations were completely erased.**


### Downleveling
---
* Instead of using template literals we use normal concatenation because template literals are part of ES5. 
* TypeScript has ability to rewrite code from newer version of ES to older version and this is called **Downleveling**.
* By default TS targets ES3. But We can choose a different version using **target** option.
* `tsc --target es2015 hello.ts` -- This will change TS to target es2015.

> While the default target is ES3, the great majority of current browsers support ES2015. Most developers can therefore safely specify ES2015 or above as a target, unless compatibility with certain ancient browsers is important.

---

### Strictness

* In contrast, a lot of users prefer to have TypeScript validate as much as it can straight away, and that’s why the language provides strictness settings as well. These strictness settings turn static type-checking from a switch (either your code is checked or not) into something closer to a dial. The further you turn this dial up, the more TypeScript will check for you. This can require a little extra work, but generally speaking it pays for itself in the long run, and enables more thorough checks and more accurate tooling. When possible, a new codebase should always turn these strictness checks on.


1. `noImplicitAny`
    
    * Falling back to any is the plain JS experience anyway.
    * And sometimes TS sometimes doesn’t try to infer types for us and instead falls back to the most lenient type: any
    * But Using any defeats the purpose of using TypeScript in the first place. 
    * Turning on the noImplicitAny flag will issue an error on any variables whose type is implicitly inferred as any
        
2. `strictNullChecks`

    * The strictNullChecks flag makes handling **null** and **undefined** more explicit, and spares us from worrying about whether we forgot to handle null and undefined