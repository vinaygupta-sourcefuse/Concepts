## ✅ **What is `useParams`?**

`useParams` is a **React Router hook** that lets you **access the dynamic parts of the URL** in your component.

So when you define a **dynamic route**, like:

```
/products/:id
```

React Router matches `:id` to the part of the URL and gives you its value via `useParams`.

---

## 📌 **Basic syntax**

```js
import { useParams } from "react-router-dom";

function MyComponent() {
  const params = useParams();
  console.log(params);
  // 👉 { id: '123' } if the URL is /products/123
}
```

✅ So `useParams` returns **an object** where **each key** is a **param name** from your route.

---

## 🗂️ **Where do params come from?**

You define params in your router like this:

```jsx
import { BrowserRouter, Routes, Route } from "react-router-dom";
import ProductDetail from "./ProductDetail";

<BrowserRouter>
  <Routes>
    <Route path="/products/:id" element={<ProductDetail />} />
  </Routes>
</BrowserRouter>
```

When a user visits `/products/42`, React Router matches `:id` and passes `{ id: "42" }` to `useParams`.

---

## ✅ **Example: using the param**

```jsx
import { useParams } from "react-router-dom";

function ProductDetail() {
  const { id } = useParams();

  return <h1>Product ID: {id}</h1>;
}
```

So visiting `/products/abc` will show:

```
Product ID: abc
```

---

## 🔑 **Why is `useParams` useful?**

1️⃣ You can **fetch data** for that specific resource:

```jsx
import { useParams, useEffect, useState } from "react-router-dom";

function ProductDetail() {
  const { id } = useParams();
  const [product, setProduct] = useState(null);

  useEffect(() => {
    fetch(`/api/products/${id}`)
      .then((res) => res.json())
      .then((data) => setProduct(data));
  }, [id]);

  if (!product) return <p>Loading...</p>;

  return <h1>{product.name}</h1>;
}
```

---

2️⃣ You can use **multiple params** too:

```jsx
<Route path="/category/:categoryId/product/:productId" element={<Product />} />

// URL: /category/5/product/23

const { categoryId, productId } = useParams();
console.log(categoryId); // 5
console.log(productId);  // 23
```

---

## ⚠️ **Important: `useParams` always returns strings**

So:

```js
<Route path="/posts/:id" ... />

URL: /posts/42

const { id } = useParams();
console.log(typeof id); // 'string'
```

✅ If you need it as a number:

```js
const postId = Number(id);
```

---

## 🔄 **How is `useParams` different from `useSearchParams`?**

| Hook              | Used for                                                |
| ----------------- | ------------------------------------------------------- |
| `useParams`       | Dynamic parts of the **path**                           |
| `useSearchParams` | **Query strings** in the URL (e.g., `?page=2&sort=asc`) |

Example:

```
/products/42?page=2
```

* `42` → path param → `useParams`
* `?page=2` → query param → `useSearchParams`

---

## ✅ **When should you use `useParams`?**

Whenever you have **:something** in your route path, and you need that value in your component to:

* Fetch data (e.g., `productId`, `userId`)
* Display dynamic content
* Pass info deeper into child components

---

## 🧩 **Full mini example**

```jsx
import { BrowserRouter, Routes, Route, Link, useParams } from "react-router-dom";

function User() {
  const { userId } = useParams();
  return <h2>User ID: {userId}</h2>;
}

export default function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/user/123">User 123</Link>
      </nav>
      <Routes>
        <Route path="/user/:userId" element={<User />} />
      </Routes>
    </BrowserRouter>
  );
}
```

✅ Click the link → URL becomes `/user/123` → `useParams()` gives you `{ userId: '123' }`.

---

## 🧠 **Key takeaway**

`useParams` is your tool to **read the URL path’s dynamic parts** in a clean React way.
