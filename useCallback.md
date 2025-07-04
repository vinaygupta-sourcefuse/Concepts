## 🔄 What is `useCallback`?

`useCallback` is a **React Hook** that **memoizes a function**, meaning it will **return the same function instance** unless its dependencies change.

### 💬 In plain English:

> `useCallback` helps **prevent unnecessary re-creation of functions** between renders — especially useful when passing functions to child components or dependencies in other hooks.

---

## 🧠 Syntax

```js
const memoizedCallback = useCallback(() => {
  // your function code
}, [dependencies]);
```

* It only recreates the function **if one of the dependencies changes**.

---

## ✅ When to Use `useCallback`

### Use it when:

* You're passing **callback functions to child components** (to prevent unnecessary re-renders)
* You're using a function inside a `useEffect`, `useMemo`, or other hook that relies on **stable references**

---

## 🧪 Real Example

### 👎 Without `useCallback`:

```js
const MyComponent = () => {
  const handleClick = () => {
    console.log("Clicked!");
  };

  return <ChildComponent onClick={handleClick} />;
};
```

Every render, `handleClick` is **re-created**, which might cause `ChildComponent` to re-render even if nothing changed.

---

### ✅ With `useCallback`:

```js
const MyComponent = () => {
  const handleClick = useCallback(() => {
    console.log("Clicked!");
  }, []); // no dependencies, so it stays the same

  return <ChildComponent onClick={handleClick} />;
};
```

Now `handleClick` is **memoized**, and `ChildComponent` won’t re-render unnecessarily.

---

## 🛑 Don't Overuse It

If the function is not being passed to children or used in a dependency array, using `useCallback` can be **overkill** and even **hurt performance**.

---

## 🔁 Summary

| Feature        | `useCallback`                                |
| -------------- | -------------------------------------------- |
| Purpose        | Memoizes functions                           |
| Returns        | Same function unless dependencies change     |
| Use case       | Prevent re-renders or unstable dependencies  |
| Don't use when | Function is cheap and not causing re-renders |

---
Great question! `useCallback` and `useMemo` are **similar React hooks** that optimize performance by **memoizing** values — but they serve **different purposes**.

---

## 🔁 `useCallback` vs `useMemo`

| Feature    | `useCallback`                                    | `useMemo`                                      |
| ---------- | ------------------------------------------------ | ---------------------------------------------- |
| Returns    | A **memoized function**                          | A **memoized value** (result of a function)    |
| Used for   | Avoiding **recreating functions** on each render | Avoiding **recomputing values** on each render |
| Signature  | `useCallback(fn, deps)`                          | `useMemo(() => fn(), deps)`                    |
| Common Use | Passing stable function refs to child components | Memoizing expensive calculations               |

---

### 🧠 Think of it this way:

* Use **`useMemo`** when you want to memoize a **value**.
* Use **`useCallback`** when you want to memoize a **function**.

---

## 🔧 Examples

### ✅ `useMemo` — Memoizing a value

```jsx
const sortedList = useMemo(() => {
  return items.sort((a, b) => a - b);
}, [items]);
```

* Recomputes only when `items` changes.
* Useful if sorting is expensive.

---

### ✅ `useCallback` — Memoizing a function

```jsx
const handleClick = useCallback(() => {
  console.log('Clicked!');
}, []);
```

* Same `handleClick` reference unless dependencies change.
* Prevents unnecessary re-renders when passed to child components.

---

## 🔄 Real-world Use Case Example:

### 🔁 Without memoization:

```jsx
<ChildComponent onClick={() => doSomething()} />
```

* A **new function is created** on every render → may cause re-renders in memoized child.

---

### ✅ With `useCallback`:

```jsx
const handleClick = useCallback(() => doSomething(), []);
<ChildComponent onClick={handleClick} />
```

* `handleClick` only changes if dependencies change → more efficient.

---

### ✅ With `useMemo`:

```jsx
const filteredList = useMemo(() => {
  return list.filter(item => item.active);
}, [list]);
```

* The list is only recalculated when `list` changes.

---

## 🚨 When NOT to Use

Don’t use `useMemo` or `useCallback` *everywhere* — only use them:

* When performance is a real issue (e.g., slow renders or child components re-rendering unnecessarily).
* When the memoized result actually avoids a heavy computation or re-creation.

