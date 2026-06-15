# Shopping Cart — React Context API Project

A simple shopping cart app built with **React + Vite** to practice the **Context API** for global state management. Products are randomly generated using `@faker-js/faker`, and users can add/remove items from a cart, with the total price calculated automatically.

## Features

- Home page showing 20 randomly generated products
- Add to Cart / Remove from Cart on each product
- Cart page showing selected items and total price
- Cart count shown live in the navigation header
- Routing between Home and Cart pages using React Router

## Tech Stack

- React (Vite)
- React Router v6
- Context API (`createContext`, `useContext`, `useState`)
- `@faker-js/faker` for fake product data

## How Context API Works in This Project

Normally, to share data (like the cart) between components — Header, Home, Cart, SingleProduct — you'd have to pass it down through props from a common parent. As the app grows, this becomes messy ("prop drilling"). Context API solves this by letting any component access shared state directly, without passing it through every level.

**1. Create the context** (`Context.jsx`)

```js
const Cart = createContext();
```

This creates a "box" that can hold shared data, accessible from anywhere in the component tree.

**2. Create a Provider component**

The `Context` component holds the actual state (`cart`, `setCart`, `products`) and wraps everything in `Cart.Provider`, passing that state as the `value`:

```jsx
<Cart.Provider value={{ cart, setCart, products }}>
  {children}
</Cart.Provider>
```

**3. Wrap the app** (`main.jsx`)

```jsx
<Context>
  <App />
</Context>
```

Now every component inside `<App />` — no matter how deeply nested — can access `cart`, `setCart`, and `products`.

**4. Create a custom hook to consume it**

```js
export const CartState = () => {
  return useContext(Cart);
};
```

**5. Use it in any component**

```js
const { cart, setCart, products } = CartState();
```

- `Header.jsx` reads `cart` to show the item count
- `Home.jsx` reads `products` to display the product list
- `SingleProduct.jsx` reads and updates `cart` when Add/Remove is clicked
- `Cart.jsx` reads `cart` to display selected items and calculate the total

All four components share the **same state** without passing any props between them — that's the core benefit of Context API.


## When to Use Context API

Good for: theme, auth/user info, language, cart state — data that's needed in many places but doesn't change extremely often.

Not ideal for: very frequently-updating state across large apps (consider Redux Toolkit or Zustand instead, as they handle performance better at scale).