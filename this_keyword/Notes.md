> "`this` refers to the value determined by how the function is called."

That's the key idea behind call(), apply(), and bind().

Let's understand all three using below example.
```
function hello() {
    return "hello: " + this.name;
}

const user = {
    name: "Dhanjeet"
};
```
Here, hello is just a normal function. It isn't attached to user.

So when you do:
```
console.log(hello());
```
the important question is:

What is `this` inside hello?

It depends on the JavaScript mode.

In strict mode, `this` is `undefined`, so:
```
this.name
```
would actually throw:
```
TypeError: Cannot read properties of undefined
```
In non-strict mode, a standalone function call generally gets the global object as `this`, so `this.name` is usually `undefined` in this example.

So don't think of it as "this doesn't know what object to refer to." 

Instead:

> Nothing about the function call hello() tells JavaScript that `this` should be `user`.



### 1. call()
```
hello.call(user);
```
Think of call() as:

> "Call this function right now, and use `user` as `this`."

So Conceptually `this == user` inside that particular function invocation.

Therefore: `this.name` becomes `user.name` 

so
```
console.log(hello.call(user));
```
produces:
```
hello: Dhanjeet
```
__call() with arguments__ : This is where call() becomes more interesting.
```
function hello(greeting) {
    return greeting + ", " + this.name;
}
```
You can do 
```
hello.call(user, "Hi");
```
Here: 
```
user       → this
"Hi"       → first argument
```

### 2. apply()

apply() does essentially the same thing as call() regarding this.
```
hello.apply(user);
```
means:

> "Call `hello` right now with `user` as `this`."

So:
```
console.log(hello.apply(user));
```
gives
```
hello: Dhanjeet
```
The difference appears when you have arguments.

With call():
```
hello.call(user, "Hi", "Good morning");
```
Arguments are passed individually.

With apply():
```
hello.apply(user, ["Hi", "Good morning"]);
```
Arguments are passed as an array/array-like collection.

For example:
```
function introduce(age, city) {
    return `${this.name} is ${age} and lives in ${city}`;
}

console.log(
    introduce.call(user, 25, "Hyderabad")
);
```
versus
```
console.log(
    introduce.apply(user, [25, "Hyderabad"])
);
```
Both produce the same result.

So a simple way to remember it:
```
call()
     this, arg1, arg2, arg3

apply()
     this, [arg1, arg2, arg3]
```

### 3. Using bind()

bind() does not call the function immediately and it returns a new function.

```
const helloUser = hello.bind(user);
```
Now `helloUser` is a new function whose `this` is bound to `user`.

You can call it later:
```
console.log(helloUser());

Output: 
hello: Dhanjeet
```

