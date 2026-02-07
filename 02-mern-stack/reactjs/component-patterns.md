# React Component Patterns

## Overview

Common patterns for building scalable React applications.

---

## Component Types

### Functional Components (Preferred)

```jsx
function Welcome({ name }) {
    return <h1>Hello, {name}</h1>;
}
```

### Class Components (Legacy)

```jsx
class Welcome extends React.Component {
    render() {
        return <h1>Hello, {this.props.name}</h1>;
    }
}
```

---

## Composition vs Inheritance

React favors **composition** over inheritance.

```jsx
// Composition with children
function Card({ children, title }) {
    return (
        <div className="card">
            <h2>{title}</h2>
            <div className="card-body">{children}</div>
        </div>
    );
}

// Usage
<Card title="Welcome">
    <p>This is the card content</p>
    <Button>Click me</Button>
</Card>
```

---

## Container/Presentational Pattern

Separate logic from UI.

```jsx
// Container (Logic)
function UserListContainer() {
    const [users, setUsers] = useState([]);
    const [loading, setLoading] = useState(true);
    
    useEffect(() => {
        fetchUsers().then(data => {
            setUsers(data);
            setLoading(false);
        });
    }, []);
    
    return <UserList users={users} loading={loading} />;
}

// Presentational (UI)
function UserList({ users, loading }) {
    if (loading) return <Spinner />;
    return (
        <ul>
            {users.map(user => (
                <li key={user.id}>{user.name}</li>
            ))}
        </ul>
    );
}
```

---

## Higher-Order Components (HOC)

Function that takes a component and returns a new component.

```jsx
function withAuth(WrappedComponent) {
    return function AuthenticatedComponent(props) {
        const { isAuthenticated } = useAuth();
        
        if (!isAuthenticated) {
            return <Navigate to="/login" />;
        }
        
        return <WrappedComponent {...props} />;
    };
}

// Usage
const ProtectedDashboard = withAuth(Dashboard);
```

---

## Render Props

Share code between components using a prop whose value is a function.

```jsx
function Mouse({ render }) {
    const [position, setPosition] = useState({ x: 0, y: 0 });
    
    const handleMouseMove = (e) => {
        setPosition({ x: e.clientX, y: e.clientY });
    };
    
    return (
        <div onMouseMove={handleMouseMove}>
            {render(position)}
        </div>
    );
}

// Usage
<Mouse render={({ x, y }) => (
    <p>Mouse position: {x}, {y}</p>
)} />
```

---

## Custom Hooks (Modern Approach)

Replace HOCs and render props with hooks.

```jsx
function useWindowSize() {
    const [size, setSize] = useState({
        width: window.innerWidth,
        height: window.innerHeight
    });
    
    useEffect(() => {
        const handleResize = () => {
            setSize({
                width: window.innerWidth,
                height: window.innerHeight
            });
        };
        
        window.addEventListener('resize', handleResize);
        return () => window.removeEventListener('resize', handleResize);
    }, []);
    
    return size;
}

// Usage
function Component() {
    const { width, height } = useWindowSize();
    return <p>Window: {width} x {height}</p>;
}
```

---

## Controlled vs Uncontrolled Components

### Controlled (React manages state)

```jsx
function ControlledInput() {
    const [value, setValue] = useState('');
    
    return (
        <input 
            value={value}
            onChange={(e) => setValue(e.target.value)}
        />
    );
}
```

### Uncontrolled (DOM manages state)

```jsx
function UncontrolledInput() {
    const inputRef = useRef();
    
    const handleSubmit = () => {
        console.log(inputRef.current.value);
    };
    
    return <input ref={inputRef} />;
}
```

---

## Compound Components

Components that work together sharing implicit state.

```jsx
const Toggle = ({ children }) => {
    const [on, setOn] = useState(false);
    const toggle = () => setOn(!on);
    
    return React.Children.map(children, child => {
        if (React.isValidElement(child)) {
            return React.cloneElement(child, { on, toggle });
        }
        return child;
    });
};

Toggle.On = ({ on, children }) => (on ? children : null);
Toggle.Off = ({ on, children }) => (on ? null : children);
Toggle.Button = ({ on, toggle }) => (
    <button onClick={toggle}>{on ? 'ON' : 'OFF'}</button>
);

// Usage
<Toggle>
    <Toggle.On>The toggle is on</Toggle.On>
    <Toggle.Off>The toggle is off</Toggle.Off>
    <Toggle.Button />
</Toggle>
```

---

## Props Pattern

### Default Props

```jsx
function Button({ variant = 'primary', children }) {
    return <button className={variant}>{children}</button>;
}
```

### Spreading Props

```jsx
function Button({ className, ...rest }) {
    return <button className={`btn ${className}`} {...rest} />;
}
```

---

## Lazy Loading

```jsx
import { lazy, Suspense } from 'react';

const HeavyComponent = lazy(() => import('./HeavyComponent'));

function App() {
    return (
        <Suspense fallback={<Loading />}>
            <HeavyComponent />
        </Suspense>
    );
}
```

---

## Error Boundaries

```jsx
class ErrorBoundary extends React.Component {
    state = { hasError: false };
    
    static getDerivedStateFromError(error) {
        return { hasError: true };
    }
    
    componentDidCatch(error, errorInfo) {
        logErrorToService(error, errorInfo);
    }
    
    render() {
        if (this.state.hasError) {
            return <h1>Something went wrong.</h1>;
        }
        return this.props.children;
    }
}

// Usage
<ErrorBoundary>
    <MyComponent />
</ErrorBoundary>
```

---

## Resources

- [React Patterns](https://reactpatterns.com/)
- [React Docs](https://react.dev/)
