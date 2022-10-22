# Optimization: by caching expensive operations

We would often encounter having a function that does some intensive and expensive operations such as the following:

```tsx
const expensiveCalculation = (num:number) => {
  console.log("Calculating...");
  for (let i = 0; i < 1000000000; i++) {
    num += 1;
  }
  return num;
};

const calculation = expensiveCalculation(count);
```

When this function is part of a component, that re-renders quite often then we'll get into a big trouble when it comes to performance. Here's where [**React.useMemo**](https://reactjs.org/docs/hooks-reference.html#usememo) hook will come in handy. **useMemo** can be used to keep expensive, resource intensive functions from needlessly running.

```tsx
const [count, setCount] = useState(0);

// const calculation = expensiveCalculation(count); ❌
const calculation = useMemo(() => expensiveCalculation(count), [count]); // ✅
```

By declaring **expensiveCalculation** inside a **useMemo** hook, expensiveCalculation will then only execute when the **count** state has changed instead of every re-render.
