In TypeScript, decorators are a feature that allow you to attach metadata to classes, methods, properties, or parameters at design time. They are a form of syntactic sugar for higher-order functions applied to classes, methods, accessors, properties, or parameters. Decorators are extensively used in libraries like Angular and for various purposes such as logging, validation, dependency injection, etc.

To create a custom decorator in TypeScript, you use the `@decoratorName` syntax followed by the target element (e.g., class, method, property). Decorators are functions that receive either three or two arguments, depending on whether they are applied to a method, accessor, property, or parameter.

Here's an example of how to create a custom decorator:

```typescript
// Custom decorator function
function log(target: any, key: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;

  // Wrapping the original method with additional logging functionality
  descriptor.value = function (...args: any[]) {
    console.log(`Executing ${key} with arguments: ${args}`);
    return originalMethod.apply(this, args);
  };

  return descriptor;
}

// Example usage of the decorator
class MyClass {
  @log
  greet(name: string) {
    console.log(`Hello, ${name}!`);
  }
}

// Creating an instance of MyClass
const myObj = new MyClass();

// Calling the decorated method
myObj.greet("Alice");
```

In this example:

- We define a custom decorator function `log`. It takes three parameters: `target`, `key`, and `descriptor`. Here, `target` refers to the prototype of the class, `key` is the name of the decorated method, and `descriptor` is a property descriptor object that contains information about the method.
- Inside the decorator, we wrap the original method with additional logging functionality. We then assign this wrapped method back to the `descriptor.value`.
- We apply the `@log` decorator to the `greet` method of the `MyClass` class.
- When `myObj.greet('Alice')` is called, it will log the method name and arguments before executing the original method.

You can create more complex decorators based on your specific requirements, such as decorators with parameters, class decorators, or property decorators. Each type of decorator receives different arguments and has its own use cases.
