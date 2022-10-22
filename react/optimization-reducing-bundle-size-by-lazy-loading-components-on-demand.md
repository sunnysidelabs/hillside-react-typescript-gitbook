# Optimization: reducing bundle size by lazy loading components on-demand

****[**Lazy loading**](https://blog.logrocket.com/understanding-lazy-loading-in-javascript/) is a design pattern for optimizing web and mobile apps. The concept of lazy loading is simple: initialize objects that are critical to the user interface first and quietly render noncritical items later.

When you visit a website or use an app, it’s very likely you’re not seeing all the available content. Depending on how you navigate and use the app, you may never encounter the need for certain components, and loading unneeded items costs time and computing resources. Lazy loading enables you to render elements on demand, making your app more efficient and improving the user experience.

You can lazy load a component by using React's [**lazy**](https://reactjs.org/docs/react-api.html#reactlazy) and [**Suspense**](https://reactjs.org/docs/react-api.html#reactsuspense) such as the following:

```tsx
import { lazy, Suspense } from "react";

// This component will be loaded on-demand
const OtherComponent = lazy(() => import('./OtherComponent'));

const MyComponent = () => {
  return (
    // Displays <Spinner> until OtherComponent loads
    <Suspense fallback={<Spinner />}>
      <div>
        <OtherComponent />
      </div>
    </Suspense>
  );
}
```

A component created using [**React.lazy()**](https://reactjs.org/docs/react-api.html#reactlazy) is loaded only when it needs to be rendered. Therefore, you should display some kind of placeholder content while the lazy component is being loaded , such as a loading indicator. This is exactly what [**React.Suspense**](https://reactjs.org/docs/react-api.html#reactsuspense) is designed for.

****[**React.Suspense**](https://reactjs.org/docs/react-api.html#reactsuspense) is a component for wrapping lazy components. You can wrap multiple lazy components at different hierarchy levels with a single `Suspense` component.

The [**Suspense**](https://reactjs.org/docs/react-api.html#reactsuspense) component takes a **fallback** prop that accepts the React elements you want rendered as placeholder content while all the lazy components get loaded.

It is also wise to place an error boundary anywhere above a lazy component to enhance the user experience in the event that a lazy component fails to load.

```tsx
const MyComponent = () => {
  return (
    <ErrorBoundary>
      // Displays <Spinner> until OtherComponent loads
      <Suspense fallback={<Spinner />}>
        <div>
          <OtherComponent />
        </div>
      </Suspense>
    </ErrorBoundary>
  );
}
```

For frameworks such as [**Next.js**](https://nextjs.org/), there are a couple of ways to lazy-load or dynamically import components, please refer to their documentation [**here**](https://nextjs.org/docs/advanced-features/dynamic-import).



**References:**

{% embed url="https://nextjs.org/docs/advanced-features/dynamic-import" %}

{% embed url="https://blog.logrocket.com/lazy-loading-components-in-react-16-6-6cea535c0b52" %}
