### 🧠 `useState` in React

`useState` is a Hook in React that lets you **add state to function components**. It's the most basic way to store and update data within a component.

---

### ✅ Syntax

```jsx
const [state, setState] = useState(initialValue);
```

* `state`: The current value.
* `setState`: A function to update the value.
* `initialValue`: The default value of the state.

---

### 🧪 Example

```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0); // count starts at 0

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

In this example:

* The button click updates the `count` using `setCount`.
* React re-renders the component with the new `count`.

---

### 🔄 Updating State

* **With new value:**

  ```jsx
  setCount(5);
  ```

* **With a function (recommended if based on previous state):**

  ```jsx
  setCount(prev => prev + 1);
  ```

---

### 💡 Notes

* You can call `useState` multiple times for multiple variables.
* React batches state updates and re-renders accordingly.
* `useState` is **asynchronous** — updating it won't immediately reflect in the next line of code.

---

Would you like an example of managing multiple states or using `useState` with objects or arrays?
