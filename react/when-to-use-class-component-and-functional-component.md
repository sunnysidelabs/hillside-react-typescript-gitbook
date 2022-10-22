# When to use Class Component and Functional Component?

In version [**React 16.8**](https://reactjs.org/blog/2019/02/06/react-v16.8.0.html) Hooks were introduced and with the use of [**useState**](https://reactjs.org/docs/hooks-reference.html#usestate) hook we can use the state in **Functional Components** as well. It is now advised to use **Functional Components** rather than **Class Components** even by Facebook itself.

It is good practice to always use **Functional Components** since it allows developers to write cleaner and more concise codes than a **Class Component**. However, one danger about functional components is that developers need to wisely code their functional components in order to [**prevent unnecessary re-render**](https://paul-cham.gitbook.io/hillside/react/optimization-functional-components-by-preventing-wasted-renders). Otherwise, it is better to stay with class components so developers can control the render imperatively throughout its life cycle.

The only reason to use **Class Components** with modern React is when [**Error Boundaries**](https://reactjs.org/docs/error-boundaries.html) are needed. Error boundaries are React components that **catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI** instead of the component tree that crashed. Error boundaries catch errors during rendering, in lifecycle methods, and in constructors of the whole tree below them.

Even though, the official React team has not provided error boundary support for functional components, we can still achieve error boundary on functional component by using [**react-error-boundary**](https://github.com/bvaughn/react-error-boundary) package.

To summarize, always use **Functional Components**, but be wary of unnecessary re-renders. Make use of [**`React.memo`**](https://reactjs.org/docs/react-api.html#reactmemo) and ensure that the component props callback functions are referenced only once by using [**`useCallback`**](https://reactjs.org/docs/hooks-reference.html#usecallback). In the case that [**Error Boundaries**](https://reactjs.org/docs/error-boundaries.html) **** are needed, use the [**react-error-boundary**](https://github.com/bvaughn/react-error-boundary) package.

