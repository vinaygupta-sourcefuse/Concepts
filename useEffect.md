### 🔄 `useEffect` in React

`useEffect` is a React Hook used to **perform side effects** in function components — things like fetching data, updating the DOM, setting up subscriptions, or timers.

---

### 🧠 Syntax

```jsx
useEffect(() => {
  // side effect logic

  return () => {
    // cleanup (optional)
  };
}, [dependencies]);
```

---

### 📌 How It Works

* **Runs after render** (not during).
* The effect re-runs only if any **dependencies** in the array change.
* If the dependency array is:

  * **Omitted**: Runs after **every** render.
  * **Empty (`[]`)**: Runs **only once** on mount.
  * **\[x, y]**: Runs on mount + when **x or y change**.

---

### ✅ Example 1 – Run Once (on mount)

```jsx
useEffect(() => {
  console.log('Component mounted');
}, []);
```

---

### ✅ Example 2 – Run When Prop/State Changes

```jsx
useEffect(() => {
  console.log(`Count changed to ${count}`);
}, [count]);
```

---

### ✅ Example 3 – Cleanup (e.g. Unsubscribe)

```jsx
useEffect(() => {
  const id = setInterval(() => {
    console.log('Tick');
  }, 1000);

  return () => {
    clearInterval(id);
    console.log('Cleanup');
  };
}, []);
```

---

### ⚠️ Common Use Cases

* Fetching data (like `fetch` or Axios)
* Setting up subscriptions (e.g. WebSocket, event listeners)
* Animations or timers
* Manual DOM manipulations

---

Would you like to see an example that fetches data using `useEffect`?
