---
layout: post
title:  "JavaScript: Understand \"this\" for arrow functions"
date:   2020-01-04 23:00:00 +1100
categories: development javascript
---

We know that arrow functions don’t have their own `this` value. Instead, they remember the value of the `this` parameter at the time of their *definition*. This means when arrow functions are being used as callbacks for DOM events, they don't need to be bound with the `this` first, which is handy.

But what does "*remember the value of the `this` parameter at the time of their definition*" mean? The following code may help you understand it:

```javascript
const x = {
  name: 'Rose',
  a: function() {
    return {
      name: 'John',
      hello() {
        console.log(`Hello, ${this.name}`);
      },

      hi: () => {
        console.log(`Hi, ${this.name}`);
      },

      howAreYou: function() {
        console.log(`How are you, ${this.name}`);
      },
    };
  },
};

const o = x.a('Tom');
o.hello();
o.hi();
o.howAreYou();
```

The output will be:

```
Hello, John
Hi, Rose
How are you, John
```


