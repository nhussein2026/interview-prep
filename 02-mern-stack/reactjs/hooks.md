# React Hooks

## Overview

Hooks let you use state and other React features in functional components. Introduced in React 16.8.

---

## useState

Manage state in functional components.

```jsx
import { useState } from 'react';

function Counter() {
    const [count, setCount] = useState(0);
    
    return (
        <div>
            <p>Count: {count}</p>
            <button onClick={() => setCount(count + 1)}>Increment</button>
            <button onClick={() => setCount(prev => prev - 1)}>Decrement</button>
        </div>
    );
}
```

### Key Points
- Initial value only used on first render
- Use callback form `setCount(prev => prev + 1)` when new state depends on old
- State updates are **asynchronous** and batched

---

## useEffect

Handle side effects (data fetching, subscriptions, DOM manipulation).

```jsx
import { useState, useEffect } from 'react';

function UserProfile({ userId }) {
    const [user, setUser] = useState(null);
    
    useEffect(() => {
        // Side effect
        fetchUser(userId).then(data => setUser(data));
        
        // Cleanup function (optional)
        return () => {
            // Runs before next effect or unmount
        };
    }, [userId]); // Dependency array
    
    return <div>{user?.name}</div>;
}
```

### Dependency Array Behavior

| Dependency | When Effect Runs |
|------------|------------------|
| `[]` | Only on mount |
| `[value]` | On mount + when value changes |
| No array | Every render (avoid!) |

---

## useContext

Share data without prop drilling.

```jsx
import { createContext, useContext } from 'react';

// Create context
const ThemeContext = createContext('light');

// Provider component
function App() {
    return (
        <ThemeContext.Provider value="dark">
            <Toolbar />
        </ThemeContext.Provider>
    );
}

// Consumer component
function Button() {
    const theme = useContext(ThemeContext);
    return <button className={theme}>Click me</button>;
}
```

---

## useReducer

Alternative to useState for complex state logic.

```jsx
import { useReducer } from 'react';

const initialState = { count: 0 };

function reducer(state, action) {
    switch (action.type) {
        case 'increment':
            return { count: state.count + 1 };
        case 'decrement':
            return { count: state.count - 1 };
        case 'reset':
            return initialState;
        default:
            throw new Error();
    }
}

function Counter() {
    const [state, dispatch] = useReducer(reducer, initialState);
    
    return (
        <>
            Count: {state.count}
            <button onClick={() => dispatch({ type: 'increment' })}>+</button>
            <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
        </>
    );
}
```

### When to use useReducer vs useState
- **useState**: Simple, independent state values
- **useReducer**: Complex state logic, multiple sub-values, next state depends on previous

---

## useMemo

Memoize expensive computations.

```jsx
import { useMemo } from 'react';

function ExpensiveComponent({ items, filter }) {
    const filteredItems = useMemo(() => {
        return items.filter(item => item.includes(filter));
    }, [items, filter]); // Only recalculate when these change
    
    return <List items={filteredItems} />;
}
```

---

## useCallback

Memoize functions (prevent recreating on each render).

```jsx
import { useCallback } from 'react';

function Parent({ items }) {
    const handleClick = useCallback((id) => {
        console.log('Clicked:', id);
    }, []); // Empty deps = same function always
    
    return items.map(item => (
        <Child key={item.id} onClick={handleClick} />
    ));
}
```

### useMemo vs useCallback
```jsx
// These are equivalent:
useCallback(fn, deps)
useMemo(() => fn, deps)
```

---

## useRef

Access DOM elements or persist values without re-renders.

```jsx
import { useRef, useEffect } from 'react';

function TextInput() {
    const inputRef = useRef(null);
    
    useEffect(() => {
        inputRef.current.focus(); // Access DOM element
    }, []);
    
    return <input ref={inputRef} />;
}

// Persist value without re-render
function Timer() {
    const countRef = useRef(0);
    
    const increment = () => {
        countRef.current += 1; // No re-render!
    };
}
```

---

## Custom Hooks

Reusable logic extracted into functions.

```jsx
// useFetch custom hook
function useFetch(url) {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);
    
    useEffect(() => {
        setLoading(true);
        fetch(url)
            .then(res => res.json())
            .then(setData)
            .catch(setError)
            .finally(() => setLoading(false));
    }, [url]);
    
    return { data, loading, error };
}

// Usage
function Component() {
    const { data, loading, error } = useFetch('/api/users');
    if (loading) return <Spinner />;
    if (error) return <Error />;
    return <UserList users={data} />;
}
```

---

## Rules of Hooks

1. **Only call at top level** - Not in loops, conditions, or nested functions
2. **Only call in React functions** - Components or custom hooks

```jsx
// ❌ Wrong
if (condition) {
    const [value, setValue] = useState(0);
}

// ✅ Correct
const [value, setValue] = useState(0);
if (condition) {
    setValue(newValue);
}
```

---

## Common Interview Questions

1. **What's the difference between useMemo and useCallback?**
   - useMemo memoizes a value, useCallback memoizes a function

2. **Why do we need the dependency array?**
   - To tell React when to re-run the effect/recalculate the value

3. **What happens if you forget cleanup in useEffect?**
   - Memory leaks, stale closures, multiple subscriptions

4. **When would you use useReducer over useState?**
   - Complex state logic, multiple related values, predictable state updates

---

## Resources

- [React Hooks Documentation](https://react.dev/reference/react)
- [useHooks.com](https://usehooks.com/)
