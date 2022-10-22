# Large Dataset: by using infinite scroll

Just like [**Pagination**](https://paul-cham.gitbook.io/hillside/react/large-dataset-by-using-pagination), **Infinite Scroll** is another form of pagination but with automated navigation. Unlike pagination that requires the end user to navigate between pages, infinite scroll automatically loads the next set of data when the user reached a certain part of the UI (usually when reaching the bottom of a screen). Like pagination, **page numbers**, **page size**, and **offsets** still exist in infinite scroll. The main difference is that in infinite scroll, the next set of items are **appended** at the end of the list unlike pagination which shows a section of the list.

Implementing infinite scroll in our views is as follows:

1. Add dummy component to the bottom of the table, to indicate whether the component is loading more items, or has already shown all the items.
2. Add an observer to respond when the user scrolls to the bottom of the table.
3. Load the next page once the observer responds.

Which translates to the **useInfiniteScroll** hook and **List** component below:

{% code title="useInfiniteScroll.tsx" %}
```tsx
import { useState, useEffect } from 'react';

const useInfiniteScroll = (callback) => {
  const [isFetching, setIsFetching] = useState<boolean>(false);
  
  const debounce = (func, delay) => {
    let inDebounce;
    return function() {
      clearTimeout(inDebounce);
      inDebounce = setTimeout(() => {
        func.apply(this, arguments);
      }, delay);
    }
  } 

  useEffect(() => {
    window.addEventListener('scroll', debounce(handleScroll, 500));
    return () => window.removeEventListener('scroll', debounce(handleScroll, 500));
  }, []);

  useEffect(() => {
    if (!isFetching) return;
    callback(() => {
      console.log('called back');
    });
  }, [isFetching]);

  function handleScroll() {
    if (window.innerHeight + document.documentElement.scrollTop !== document.documentElement.offsetHeight || isFetching) return;
    setIsFetching(true);
  }

  return [isFetching, setIsFetching];
};

export default useInfiniteScroll;
```
{% endcode %}

{% code title="List.tsx" %}
```tsx
const List = () => {
  const [items, setItems] = useState<number[]>(Array.from(Array(30).keys(), n => n + 1));
  const [isFetching, setIsFetching] = useInfiniteScroll(fetchMoreListItems);

  const fetchMoreListItems = () => {
    setTimeout(() => {
      // do fetch more items and set to items
      setListItems(prevState => ([...prevState, ...Array.from(Array(20).keys(), n => n + prevState.length + 1)]));
      setIsFetching(false);
    }, 2000);
  }
  
  return (
    <>
      <ul>
        {items.map(item => <li>{item}</li>)}
      </ul>
      {isFetching && 'Fetching more list items...'}
    </>
  );
}
```
{% endcode %}

#### When to not use Infinite Scroll?

* When you want to track the pages, in infinite scroll it is difficult for users to keep track of where they are with large datasets, users can get lost.
* Since data is appended in infinite scroll, the more the user scrolls, the more elements are present on the screen which leads to poorer performance and poorer user experience.
