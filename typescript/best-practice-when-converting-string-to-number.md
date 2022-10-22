# Best Practice: when converting string to number

When converting a string value to that of a number, follow the best practices below:

**❌  DO NOT USE**

```typescript
// SLOWEST (88% slower)
const x:number = Math.floor("1000");

// SLOW (69% slower)
const x:number = parseInt("1000");
```

**✅  DO USE**

In most cases, it is considered good practice to use **Number()**, since it is **relatively fast** and is very easy to understand leading to **highly readable codes**.

```typescript
// FASTEST (1.06)
const x:number = "1000" * 1;

// FASTER (0.88)
const x:number = ~~"1000";

// FAST (0.87)
const x:number = +"1000";

// FAST (0.85)
const x:number = Number("1000");
```
