# Design Pattern
###  - Container-Presenter Pattern (Smart and Dumb)
The container -presenter pattern consist of two section , basically it divide the data storing component and data presenting component (also know as Smart and Dumb Component), that help to divide the two different logic, if we contain both in same component it will surely look messy. \
<br>
   ❌ Bad design (everything mixed)
```
// Products.jsx 
import React, { useEffect, useState } from "react";

export default function Products() {
  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(false);
  const [cart, setCart] = useState([]);

  useEffect(() => {
    fetchProducts();
  }, []);

  const fetchProducts = async () => {
    setLoading(true);
    const res = await fetch("https://fakestoreapi.com/products");
    const data = await res.json();
    setProducts(data);
    setLoading(false);
  };

  const addToCart = (product) => {
    setCart([...cart, product]);
  };

  if (loading) return <h2>Loading...</h2>;

  return (
    <div>
      <h2>Products ({cart.length})</h2>

      {products.map((p) => (
        <div key={p.id} style={{ border: "1px solid gray", margin: 10 }}>
          <h3>{p.title}</h3>
          <p>₹ {p.price}</p>
          <button onClick={() => addToCart(p)}>Add</button>
        </div>
      ))}
    </div>
  );
}
```
<br>
✅ Container Component (Smart)

```
// ProductsContainer.jsx

import React, { useEffect, useState } from "react";
import ProductsPresenter from "./ProductsPresenter";

export default function ProductsContainer() {
  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(false);
  const [cart, setCart] = useState([]);

  useEffect(() => {
    fetchProducts();
  }, []);

  const fetchProducts = async () => {
    setLoading(true);
    const res = await fetch("https://fakestoreapi.com/products");
    const data = await res.json();
    setProducts(data);
    setLoading(false);
  };

  const addToCart = (product) => {
    setCart((prev) => [...prev, product]);
  };

  return (
    <ProductsPresenter
      products={products}
      loading={loading}
      cartCount={cart.length}
      onAddToCart={addToCart}
    />
  );
}
```
<br>
✅ Presenter Component (Dumb)

```
// ProductsPresenter.jsx

import React from "react";

export default function ProductsPresenter({
  products,
  loading,
  cartCount,
  onAddToCart,
}) {
  if (loading) return <h2>Loading...</h2>;

  return (
    <div>
      <h2>Products ({cartCount})</h2>

      {products.map((p) => (
        <div key={p.id} style={{ border: "1px solid gray", margin: 10 }}>
          <h3>{p.title}</h3>
          <p>₹ {p.price}</p>
          <button onClick={() => onAddToCart(p)}>Add</button>
        </div>
      ))}
    </div>
  );
}
```

###  - Controlled vs uncontrolled component pattern
Uncontrolled components are useful in situations where you want to integrate React with non-React code or leverage existing DOM behavior.
```
const Form = () => {
  const nameRef = useRef();
  const nameRef = useRef();

  const onSubmit = () => {
    console.log("Name: " + nameRef.current.value);
    console.log("Email: " + emailRef.current.value);
  };

  return (
    <form onSubmit={onSubmit}>
      <input type="text" name="name" ref={nameRef} required />
      <input type="email" name="email" ref={emailRef} required />
      <input type="submit" value="Submit" />
    </form>
  );
}

export default Form
```
<br>
Controlled Components are useful in situations you need a form handling, dynamic UI updates or stateful logics, also it's better for testing and debugging.

```
const Form = () => {
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");

  const onSubmit = () => {
    console.log("Name: " + name);
    console.log("Email: " + email);
  };

  return (
    <form onSubmit={onSubmit}>
      <input
        type="text"
        name="name"
        value={name}
        onChange={(e) => setName(e.target.value)}
        required
      />
      <input
        type="email" 
        name="email" 
        value={email} 
        onChange={(e) => setEmail(e.target.value)} 
        required
      />
      <input type="submit" value="Submit" />
    </form>
  );
}

export default Form
```

### - Compound components pattern
    
### - Render Props Pattern
### - Higher- order Component
### - Custom hook pattern
### - Provider Pattern 
### - State Reducer Pattern
### - Observer/ Pub-Sub Pattern
### - Performnce Pattern
### - Slot Pattern
### - Hook Factory / Stergy pattern
