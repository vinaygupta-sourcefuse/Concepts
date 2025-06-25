### 📍 `useLocation` – React Router Hook

`useLocation` is a hook from **React Router** (v6+) that lets you **access the current location object** — including the pathname, search query, hash, and state passed through navigation.

---

### 📦 Import

```js
import { useLocation } from 'react-router-dom';
```

---

### 🧠 Usage

```jsx
const location = useLocation();
```

This gives you a `location` object like:

```js
{
  pathname: "/about",
  search: "?query=react",
  hash: "#top",
  state: { from: "/login" },
  key: "random_key"
}
```

---

### ✅ Example

```jsx
import { useLocation } from 'react-router-dom';

function AboutPage() {
  const location = useLocation();

  return (
    <div>
      <p>Current path: {location.pathname}</p>
      <p>Search query: {location.search}</p>
      <p>Hash: {location.hash}</p>
    </div>
  );
}
```

---

### 🔁 With `useNavigate` and state

You can pass custom data using `useNavigate`:

```jsx
navigate('/dashboard', { state: { from: 'login' } });
```

Then access it with `useLocation`:

```jsx
const location = useLocation();
console.log(location.state?.from); // "login"
```

---

### 📌 Common Use Cases

* Getting route info (e.g. current URL, query params).
* Accessing passed `state` between pages (e.g. after a redirect).
* Conditional rendering or behavior based on route.

---

Would you like an example of extracting query parameters using `useLocation`?
