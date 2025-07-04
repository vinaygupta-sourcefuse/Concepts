### ✅ `useMemo` in React — Meaning and Usage

`useMemo` is a **React Hook** used to **optimize performance** by **memoizing** (caching) the result of an expensive function **so it's only recalculated when necessary**.

---

### 🔧 Syntax:

```jsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

* `computeExpensiveValue(a, b)` is the function whose result you want to **memoize**.
* `[a, b]` is the **dependency array**.
* React will **only recompute** the value when `a` or `b` **change**.
* If `a` and `b` don't change, React uses the **cached value**.

---

### 🧠 When to Use `useMemo`:

Use `useMemo` when:

* You have a **slow or expensive calculation**.
* The result doesn’t need to change on every render.
* You're passing values to **memoized components** (like `React.memo`) and want to prevent unnecessary re-renders.

---

### 📌 Example:

```jsx
import { useMemo, useState } from 'react';

function App() {
  const [count, setCount] = useState(0);
  const [other, setOther] = useState(0);

  const expensiveComputation = (num) => {
    console.log("Calculating...");
    return num * 1000;
  };

  const memoizedResult = useMemo(() => expensiveComputation(count), [count]);

  return (
    <div>
      <p>Result: {memoizedResult}</p>
      <button onClick={() => setCount(count + 1)}>Increase Count</button>
      <button onClick={() => setOther(other + 1)}>Change Other</button>
    </div>
  );
}
```

### 🔍 Explanation:

* `expensiveComputation(count)` will only run **when `count` changes**.
* Changing `other` will not trigger recomputation.

---

### 🚫 Common Mistake:

If you **omit** dependencies from the dependency array, `useMemo` might not update when it should, leading to **stale or incorrect values**.
