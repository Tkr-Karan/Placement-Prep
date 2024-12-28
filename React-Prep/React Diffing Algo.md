# React Diffing Algo

The **React Diffing Algorithm** (often referred to as the **React Reconciliation Algorithm**) is a core part of React's rendering process. It helps React efficiently update the DOM by minimizing the number of changes required when the state or props of a component change. The goal is to improve performance and avoid unnecessary re-renders.

### Key Ideas of the React Diffing Algorithm

1. **Virtual DOM**: React uses a Virtual DOM (a lightweight in-memory representation of the actual DOM). When the state or props of a component change, React creates a new Virtual DOM tree and compares it with the previous one to calculate the minimal set of changes needed to update the actual DOM.
2. **Component Reconciliation**: React performs **reconciliation** to determine how to update the DOM efficiently. It compares the old Virtual DOM with the new one and calculates the **diffs** — the changes that need to be applied to the real DOM.
3. **Two Main Phases**:
    - **Render Phase**: React creates a new Virtual DOM based on the updated state or props.
    - **Commit Phase**: After comparing the new Virtual DOM with the previous one, React applies the minimal set of changes to the real DOM.

### How the Diffing Algorithm Works

The diffing algorithm works under the assumption that components are **immutable** — meaning that React assumes components don't change over time but re-render with new data. It uses heuristics to optimize the process of determining what parts of the DOM need to be updated.

1. **Component Type Comparison**:
    - **Different Types of Components**: If the component type changes (for example, from a `<div>` to a `<span>`), React will throw away the old tree and build a completely new one.
    - **Same Type of Components**: If the component type remains the same (e.g., both are `<div>` elements), React will try to reuse the old component tree and only update the parts that changed.
2. **Element Reuse**:
    - React compares the old and new trees element-by-element.
    - If the element types (tag names, attributes) are the same, React compares the **keys** (if provided). If the keys are the same, React will try to reuse the existing element.
    - If the keys are different, React will destroy the old element and create a new one.
3. **Key Prop**:
    - React uses the **key** prop (especially in lists) to uniquely identify each element and optimize rendering.
    - When an array of elements is rendered, React compares the elements' keys. If an element has the same key as a previous one, React will assume it is the same element and update it. If the key is different, it assumes that the element should be re-rendered or removed.
4. **Children Diffing**:
    - When comparing children of a component, React assumes that if the elements are of the same type (same tag name), the component should be reused.
    - If a component is a list, React will try to reorder the list items by their keys. If the keys match in the previous and new trees, the elements will be updated rather than recreated.
5. **Efficient Updates**:
    - React avoids unnecessary updates by only changing the parts of the DOM that need to be updated based on the new Virtual DOM.
    - It uses **heuristics** (like key comparison) to detect if elements can be reused or if new elements need to be created.

### Example of Diffing Algorithm in Action

Let's say you have the following list rendered by React:

```jsx
<ul>
  <li key="a">Item A</li>
  <li key="b">Item B</li>
  <li key="c">Item C</li>
</ul>

```

If the state of the list changes, for example, `Item A` is removed and replaced by `Item D`, React will compare the new Virtual DOM to the old one:

```jsx
<ul>
  <li key="b">Item B</li>
  <li key="c">Item C</li>
  <li key="d">Item D</li>
</ul>

```

- **Step 1**: React compares the new and old lists.
- **Step 2**: It finds that `Item A` is missing in the new list.
- **Step 3**: It keeps `Item B` and `Item C` (since their keys are the same), and it adds `Item D` at the end.

React does not need to destroy and recreate the entire list; it simply removes the `Item A` and adds the `Item D`, optimizing the update process.

### Performance Optimizations in Diffing

- **Batched Updates**: React batches DOM updates together to minimize reflows and repaints in the browser.
- **Reorderable Lists**: By using keys in lists, React can efficiently reorder items, minimizing the number of DOM manipulations.
- **Component-Level Reconciliation**: React checks if the component’s output has changed (via `shouldComponentUpdate`, `React.memo`, or hooks like `React.useMemo`), so it only re-renders when necessary.

### Conclusion

React’s diffing algorithm is designed to be efficient by minimizing the number of DOM manipulations required during re-renders. It achieves this through the use of the Virtual DOM, component type comparison, and clever key-based diffing for lists and trees of elements. These optimizations allow React to provide a fast and smooth user experience, even when the UI undergoes frequent updates.