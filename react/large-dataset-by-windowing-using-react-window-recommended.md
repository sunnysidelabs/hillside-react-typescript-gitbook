# Large Dataset: by windowing using react-window (RECOMMENDED)

While we can utilize [**Pagination**](https://paul-cham.gitbook.io/hillside/react/large-dataset-by-using-pagination) and **** [**Infinite Scroll**](https://paul-cham.gitbook.io/hillside/react/large-dataset-by-using-infinite-scroll) to render large datasets in our application without compromising on performance, both pagination and infinite scroll comes with its own set of limitations. [**Windowing**](https://github.com/bvaughn/react-window) is another data rendering technique to bridge the gap and get the best of both.&#x20;

With windowing, we display the data as if all the elements are loaded, but it keeps track of where the user is, and only render items currently in view. this is highly efficient since only a limited part of the data is rendered at all times leading to better performant application.

To easily implement windowing in your application, you may use [**react-window**](https://github.com/bvaughn/react-window) and [**react-virtualized-auto-sizer**](https://github.com/bvaughn/react-virtualized-auto-sizer) libraries:

```typescript
npm install --save react-window react-virtualized-auto-sizer
```

```tsx
import Autosizer from "react-virtualized-auto-sizer"
import { FixedSizeList as List } from "react-window"

return (
  <div>
    <Autosizer width="100%">
      {({ height, width }) => {
        return (
          <List 
            height={height} 
            width={width} 
            itemCount={items.length}
            itemSize={itemRowHeight}
          >
            {items}
          </List>
        )
      }}
    </Autosizer>
  </div>
);
```

#### When to not use Windowing?

* Complex implementation, adding windowing to your application will require you to know the height of the row and add styling in order for it to work. If the application renders different heights for each breakpoint, this can get ugly real quick.
* There are height and positioning constraints which will require some pre-calculation, before windowing can be implemented.
* Loss of HTML semantics such as converting tables into divs which can be hard for search engine indexers to quantify the content of the website.



**References:**

{% embed url="https://github.com/bvaughn/react-window" %}

{% embed url="https://github.com/bvaughn/react-virtualized-auto-sizer" %}
