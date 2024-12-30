# README

## What is this?

With TypeScript filtered arrays can have types that are misleading. When you have a an array that has been filtered on boolean values, the type could still show `false`, `null` and `undefined` as potential values. By creating a `Truthy` type we have an up to date reflection of the types in the filtered array.

```ts
const arr = ["foo", 123, "bar", 0, true, false, null, undefined];
const arrFiltered = arr.filter(Boolean); // const arrFiltered: (string | number | boolean | null | undefined)[]

console.log(arr); // ["foo", 123, "bar", 0, true, false, null, undefined]
console.log(arrFiltered); // ["foo", 123, "bar", true]

type Truthy<Not = false | undefined | null | "" | 0> = <V>(
  value: V
) => value is Exclude<V, Not>;

const truthy = Boolean as unknown as Truthy;

const arrFilteredWithTruthy = arr.filter(truthy); // const arrFilteredWithTruthy: (string | number | true)[]

console.log(arrFilteredWithTruthy); // ["foo", 123, "bar", true]
```

> deno index.ts
