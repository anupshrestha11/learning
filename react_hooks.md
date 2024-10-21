Here's a detailed description of each React hook along with examples:

### 1. **`useState`**
- **Description**: The `useState` hook is used to add state to functional components. It returns a state variable and a function to update that state. Unlike class components, functional components can't manage state without hooks, so `useState` provides a way to introduce local state.
  
**Example:**

```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

---

### 2. **`useEffect`**
- **Description**: The `useEffect` hook lets you perform side effects in functional components. Side effects are things like data fetching, subscriptions, or manually changing the DOM. It runs after every render by default, but you can control when it runs by passing dependencies (in the form of an array) as the second argument.
  
**Example:**

```jsx
import { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  }, [count]); // Only re-run the effect if count changes

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

---

### 3. **`useContext`**
- **Description**: The `useContext` hook is used to consume React's Context API, which allows you to share state across the component tree without passing props down manually at every level.

**Example:**

```jsx
import React, { useContext } from 'react';

const ThemeContext = React.createContext('light');

function ThemedButton() {
  const theme = useContext(ThemeContext);
  return <button className={theme}>Button</button>;
}

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <ThemedButton />
    </ThemeContext.Provider>
  );
}
```

---

### 4. **`useReducer`**
- **Description**: The `useReducer` hook is an alternative to `useState` for managing complex state logic. It works similarly to how a reducer works in Redux. The reducer function takes the current state and an action, and returns the new state.

**Example:**

```jsx
import { useReducer } from 'react';

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </div>
  );
}
```

---

### 5. **`useRef`**
- **Description**: The `useRef` hook is used to create a reference to a DOM element or store a value that persists across renders without causing a re-render when changed. Commonly used for accessing DOM elements directly (like input fields) or storing mutable values.

**Example:**

```jsx
import { useRef } from 'react';

function TextInputWithFocusButton() {
  const inputEl = useRef(null);

  const onButtonClick = () => {
    inputEl.current.focus();
  };

  return (
    <div>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </div>
  );
}
```

---

### 6. **`useMemo`**
- **Description**: The `useMemo` hook is used to optimize performance by memoizing the result of an expensive calculation and only recomputing it when its dependencies change. This avoids unnecessary recalculations on every render.

**Example:**

```jsx
import { useMemo, useState } from 'react';

function ExpensiveComponent({ num }) {
  const [count, setCount] = useState(0);

  const expensiveComputation = useMemo(() => {
    console.log("Calculating...");
    return num * 2;
  }, [num]);

  return (
    <div>
      <p>Count: {count}</p>
      <p>Expensive computation result: {expensiveComputation}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

---

### 7. **`useCallback`**
- **Description**: The `useCallback` hook is used to memoize a callback function so that it is not recreated on every render. It is particularly useful when passing callback functions to child components that rely on referential equality to prevent unnecessary re-renders.

**Example:**

```jsx
import { useCallback, useState } from 'react';

function Button({ onClick }) {
  return <button onClick={onClick}>Click me</button>;
}

function App() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    setCount(prevCount => prevCount + 1);
  }, []);

  return (
    <div>
      <p>Count: {count}</p>
      <Button onClick={handleClick} />
    </div>
  );
}
```

---

### 8. **`useLayoutEffect`**
- **Description**: The `useLayoutEffect` hook is similar to `useEffect`, but it runs synchronously after all DOM mutations. This means it runs before the browser has a chance to paint, making it useful when you need to measure DOM elements or make layout changes before the user sees them.

**Example:**

```jsx
import { useLayoutEffect, useRef } from 'react';

function LayoutEffectExample() {
  const divRef = useRef();

  useLayoutEffect(() => {
    divRef.current.style.color = 'red';
  }, []);

  return <div ref={divRef}>This text will be red</div>;
}
```

---

### 9. **`useImperativeHandle`**
- **Description**: The `useImperativeHandle` hook allows you to customize the instance value that is exposed to parent components when using `ref`. This is useful for making the ref more flexible, as you can control what parts of the component are exposed.

**Example:**

```jsx
import { forwardRef, useImperativeHandle, useRef } from 'react';

const FancyInput = forwardRef((props, ref) => {
  const inputRef = useRef();

  useImperativeHandle(ref, () => ({
    focus() {
      inputRef.current.focus();
    }
  }));

  return <input ref={inputRef} />;
});

function App() {
  const inputRef = useRef();

  return (
    <div>
      <FancyInput ref={inputRef} />
      <button onClick={() => inputRef.current.focus()}>Focus input</button>
    </div>
  );
}
```

---

### 10. **`useDebugValue`**
- **Description**: The `useDebugValue` hook is primarily used in custom hooks to display a label in React DevTools for easier debugging. It is rarely used directly but is handy for improving the dev experience when debugging custom hooks.

**Example:**

```jsx
import { useDebugValue, useState, useEffect } from 'react';

function useCustomHook(val) {
  const [value, setValue] = useState(val);
  
  useDebugValue(value > 5 ? 'Greater than 5' : '5 or less');

  useEffect(() => {
    setValue(val);
  }, [val]);

  return value;
}
```

---

### **Custom Hook Example: `useFetch`**
- **Description**: A custom hook encapsulates reusable logic and can be used across multiple components. It allows developers to abstract complex logic like data fetching or subscriptions, and make it easier to reuse the same behavior in different places.

**Example:**

```jsx
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const fetchData = async () => {
      setLoading(true);
      try {
        const response = await fetch(url);
        const result = await response.json();
        setData(result);
      } catch (error) {
        console.error('Error fetching data:', error);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return { data, loading };
}

// Using the custom hook
function App() {
  const { data, loading } = useFetch('https://jsonplaceholder.typicode.com/todos/1');

  if (loading) {
    return <div>Loading...</div>;
  }

  return <div>{data ? data.title : 'No data available'}</div>;
}
```

---

These descriptions and examples show the functionality

 of each hook and how they simplify tasks like managing state, side effects, and optimizing performance in React functional components.