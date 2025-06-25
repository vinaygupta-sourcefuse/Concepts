### 🧠 Syntax

```jsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

---

### 🔍 Parameters

1. **Function**: `() => computeExpensiveValue(a, b)`

   * This is the **function** that returns the value you want to memoize.
2. **Dependency array**: `[a, b]`

   * Recalculates only if one of the dependencies changes.

---

### 📦 Example

```jsx
import { useMemo } from 'react';

function ExpensiveComponent({ items }) {
  const sortedItems = useMemo(() => {
    console.log('Sorting items...');
    return items.sort((a, b) => a - b);
  }, [items]);

  return (
    <ul>
      {sortedItems.map(item => <li key={item}>{item}</li>)}
    </ul>
  );
}
```

In this example:

* The list is only re-sorted if `items` changes.
* This avoids unnecessary re-sorting on every render.

---

### ⚠️ When to Use

* Use it for **expensive computations** that shouldn't rerun every render.
* **Don't overuse** it — unnecessary memoization can lead to complex and hard-to-maintain code.

---

Would you like an example comparing performance with and without `useMemo`?
