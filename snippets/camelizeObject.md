---
title: Camelize object
tags: object
author: JoelRomero97
cover: blog_images/campfire.jpg
firstSeen: 2023-01-24T16:10:13+0000
---

This method transforms an object from snake case to camel case, recursively, working with nested attributes.

- It uses [toCamelCase](/snippets/toCamelCase.md) method as auxiliar to transform keys to camel case.
- If the value is array, will `camelize` all entries
- If the value is an object, will `camelize` every single key

```js
const toCamelCase = str => {
    const s =
        str &&
        str
            .match(
                /[A-Z]{2,}(?=[A-Z][a-z]+[0-9]*|\b)|[A-Z]?[a-z]+[0-9]*|[A-Z]|[0-9]+/g
            )
            .map(x => x.slice(0, 1).toUpperCase() + x.slice(1).toLowerCase())
            .join('');
    return s.slice(0, 1).toLowerCase() + s.slice(1);
};

const camelizeObject = (obj) => {
    if (Array.isArray(obj)) {
        return obj.map(v => camelizeObject(v));
    } else if (obj !== null && obj.constructor === Object) {
        return Object.keys(obj).reduce(
            (result, key) => ({
                ...result,
                [toCamelCase(key)]: camelizeObject(obj[key]),
            }),
            {},
        );
    }
    return obj;
};
```

```js
camelizeObject({first_key: true, nested_object: [{second_key: 2}]}); // {firstKey: true, nestedObject: [{secondKey: 2}]}
```
