# DON'T: use index as a key if the order of the items in the list can change

When rendering a list of items in **React**, it is required to set a **key** prop for each child item to be rendered. Many developers use the **index** of an item as its **key** when they render a list. That is fine in some cases, it looks elegant and it does get rid of the warning (which was the 'real' issue, right?) but on another side, what is the danger here?

Since React knows each child item by its key: **index**, what if the list got reordered? item at index zero can now be item at index one. **It may break your application and display wrong data!**

It is not recommend using indexes for keys if the order of items may change doing so can **negatively impact performance** and may **cause issues with component state**. If there are no stable IDs for rendered items that can be used, you may use the item index as a key as a **last resort**.
