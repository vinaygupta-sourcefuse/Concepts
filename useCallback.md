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

Let me know if you want to see how `useCallback` compares to `useMemo`, or want to test with a live React example!
