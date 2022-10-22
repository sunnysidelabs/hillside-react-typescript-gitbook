# Optimization: by preventing wasted renders on Complex Props

When dealing with **Functional Components** with complex props, we can utilize [**`React.memo`**](https://reactjs.org/docs/react-api.html#reactmemo) and do props comparison to handle re-renders such as the following:

```tsx
const FunctionalComponent = (props) => {
  /* render using props */
}

const compareProps = (prevProps, nextProps) => {
  /*
  return true if passing nextProps to render would return
  the same result as passing prevProps to render,
  otherwise return false
  */
  return prevProps.items.length === nextProps.items.length;
}

export default React.memo(FunctionalComponent, compareProps);
```

One thing to note is that in order for the comparison to work, the function must be working with **Immutable Data**. The idea behind using immutable data structures is simple. For complex data-types the comparison performs over their reference. Whenever an object containing complex data changes, instead of making the changes in that object, we can create a copy of that object with the changes which will create a new reference so [**`React.memo`**](https://reactjs.org/docs/react-api.html#reactmemo) can perform its props comparison.

```tsx
const [items, setItems] = useState<string[]>();

<FunctionalComponent items={items} />
```



So when we're changing the state of items, we should be working with **immutable** **data**. **** Luckily, to make things easy for us, **ES6** has an object spreading operator to help us create immutable data with ease.&#x20;

✅  **Do use Immutable Data**

```tsx
const removeItem = (id) => {
  const clonedItems = { ...items };
  delete clonedItems[id];
  
  setItems(clonedItems);
};
```

and not this, since the following is dealing with a **mutable** data **items**.

❌  **Do not use** **Mutable Data**

```tsx
const removeItem = (id) => {
  delete items[id];
  
  setItems({ ...items });
};
```



**References:**

{% embed url="https://www.freecodecamp.org/news/how-to-identify-and-resolve-wasted-renders-in-react-cc4b1e910d10" %}
