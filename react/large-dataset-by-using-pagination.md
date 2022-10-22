# Large Dataset: by using pagination

**Pagination** is a technique used by most applications to deal with large datasets. **Pagination is the process in which large datasets are split into multiple smaller datasets called pages**. It is important to note that for a given page and a given dataset, there should be consistent ordering. Meaning, a given page of a dataset should return similar results. Pagination is also a technique found in modern APIs to actually allow developers to lazy-load data fetch requests, saving the end-user bandwidth which ultimately leads to a better user experience.

When using **Pagination** we usually define the following **common terms**:

1. **Page Size** - Maximum size for any chunk of the given dataset
2. **Offset** - How many data points to skip before showing the current data chunk
3. **Page** - The current chunk of data being displayed

Implementing pagination in our views is as follows:

1. Keep track of the **page size** and the current **offset**.
2. Slice or extract current page from the dataset and render them.
3. Add page navigation controls

Which translates to the pagination module such as the following:

```tsx
const getPage = (n:number) => {
  const offset:number = n * pageSize;
  return data.slice(offset, offset + pageSize);
}

const getTotalPages = () => {
  return Math.ceil(data.length / pageSize);
}
```

And a simple page navigation controls component:

```tsx
const PageNavigation = ({ 
  nextPageHandler, 
  previousPageHandler, 
  currentPage, 
  totalPages 
}) => {
  return (
    <div>
      {currentPage === 0 ? null : (
        <button onClick={previousPageHandler}>Previous</button>
      )}
      
      {currentPage + 1 >= totalPages ? null : (
        <button onClick={nextPageHandler}>Next</button>
      )}
      
      <span>
        Page {currentPage + 1} of {totalPages}
      </span>
    </div>
  )
}
```

#### When to not use Pagination?

* When you want the UI to be seamless, [**Infinite Scroll**](https://paul-cham.gitbook.io/hillside/react/large-dataset-by-using-infinite-scroll) is the better option allowing seamless navigation as opposed to pagination that requires the user to keep on navigating between pages to see new data.
* Page navigation component can look very clunky especially when displaying extremely large datasets containing thousands of pages, especially on smaller devices with smaller screens.
