# Optimization: Functional Components by preventing wasted renders

When declaring a **`Functional Component`**, such as the following:

```tsx
export const Button = ({ onClick }) => {
  return (
    <button
      onClick={onClick}
    >
      Add
    </button>
  );
};
```

It is important to note that this component will re-render all the time, which doesn't make sense because this component is just a **Button** with an **`onClick`** handler. By using [**React Developer Tools**](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en) and [**profiling**](https://reactjs.org/docs/optimizing-performance.html#profiling-components-with-the-chrome-performance-tab) **** on Chrome, we can verify that **Button** indeed re-renders. **** There is no need to keep on re-rendering this component, since it will always display the same thing.&#x20;

![Uh-oh! Button gets re-rendered more than once](../.gitbook/assets/react-wasted-render-simple-component-problem.png)

In order for React to know that the **Functional Component** should be memoized and pure, we can use [**`React.memo`**](https://reactjs.org/docs/react-api.html#reactmemo). So when do we use [**`React.memo`**](https://reactjs.org/docs/react-api.html#reactmemo)?

1. **Pure Functional Component** - The **Component** is functional and given the same props, will always renders the same output.
2. **Renders often** - The **Component** renders often.
3. **Re-renders with the same props** - The **Component** is usually provided with the same props during re-rendering.
4. **Medium to big size** - The **Component** contains a decent amount of UI elements to reason props equality check.

Knowing that, let's convert the same **Functional Component** but this time using [**`React.memo`**](https://reactjs.org/docs/react-api.html#reactmemo):

```tsx
const _Button = ({ onClick }) => {
  return (
    <button
      onClick={onClick}
    >
      Add
    </button>
  );
};

export const Button = React.memo(_Button);
```

**HOWEVER**, declaring this button component with [**`React.memo`**](https://reactjs.org/docs/react-api.html#reactmemo) won't magically solve the problem of wasted rendering, if the parent component provides different instances of the callback function on every render such as the following:

```tsx
// Arrow function creates a new setIsOpen function everytime
<Button onClick={() => setIsOpen(true)} />
```

Even if provided with the same **`onClick`** value, **Button** renders every time because it receives new instances of **`onClick`** callback. To tackle this problem, the handler for **`onClick`** should only be created once. React comes with [**`useCallback`**](https://reactjs.org/docs/hooks-reference.html#usecallback) hook which creates a memoized callback, that only changes if one of the dependencies changed, similar to [**`useEffect`**](https://reactjs.org/docs/hooks-reference.html#useeffect). By utilizing this technique, we can achieve the same thing but effectively prevent the wasted rendering.

```tsx
const showDialog = useCallback(() => setIsOpen(true), []);

<Button onClick={showDialog} />
```

Again, by using [**React Developer Tools**](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en) and [**profiling**](https://reactjs.org/docs/optimizing-performance.html#profiling-components-with-the-chrome-performance-tab) on Chrome, we can verify that **Button** no longer re-renders, which means that there are no more wasted rendering.

![Hooray! Button no longer re-renders](../.gitbook/assets/react-wasted-render-simple-component-fixed.png)

[**`React.memo`**](https://reactjs.org/docs/react-api.html#reactmemo) is a great tool to memoize **Functional** **Components**. When applied correctly, it prevents useless re-renderings when the next props equal to previous ones.

**Take precautions when memoizing components that use props as callbacks. Make sure to provide the same callback function instance between renderings.**

Don't forget to use [**profiling**](https://reactjs.org/docs/optimizing-performance.html#profiling-components-with-the-chrome-performance-tab) to measure the performance gains of memoization.



**References:**

{% embed url="https://dmitripavlutin.com/use-react-memo-wisely" %}

{% embed url="https://www.freecodecamp.org/news/how-to-identify-and-resolve-wasted-renders-in-react-cc4b1e910d10" %}
