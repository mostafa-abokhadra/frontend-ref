# React

## installation
`npm create vite@latest`\
then answer the questions => `yes`, and `name your project`\
then select the framework e:i `react`\
go to project folder created automatically named with your "project name" `cd projectName"`\
and then install all of your dependencies `using npm i`\
`code .`\
and now to run your web server `npm run dev`
## componenet
- React apps are made out of components
- A component is a piece of the UI (user interface) that has its own logic and appearance.
- A component can be as small as a button, or as large as an entire page.
- React components are JavaScript functions that return markup
- React component names must always start with a capital letter,
```js
function MyButton() {
  return (
    <button>
      I'm a button
    </button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton />
    </div>
  );
}
```

## JSX
- The markup syntax you’ve seen above is called JSX
- JSX is stricter than HTML
- You have to close tags like <br />
- Your component also can’t return multiple JSX tags
- You have to wrap them into a shared parent, like a <div>...</div> or an empty <>...</> wrapper:

## styling
- add class attribute to an element using the attribute `className=""`

## dynamic attibutes
```js
      <img
        className="avatar"
        src={user.imageUrl}
        alt={'Photo of ' + user.name}
        style={{
          width: user.imageSize,
          height: user.imageSize
        }}
      />
```
In the above example, style={{}} is not a special syntax, but a regular {} object inside the style={ } JSX curly braces. You can use the style attribute when your styles depend on JavaScript variables.
```js
    const listItems = products.map(product =>
        <li key={product.id}>
            {product.title}
        </li>
    );

    return (
    <ul>{listItems}</ul>
    );
```
key attribute. For each item in a list, you should pass a string or a number that uniquely identifies that item among its siblings.

## conditional rendering
there is no special syntax for writing conditions. Instead, you’ll use the same techniques as you use when writing regular JavaScript code. 

```js
let content;
if (isLoggedIn) {
  content = <AdminPanel />;
} else {
  content = <LoginForm />;
}
return (
  <div>
    {content}
  </div>
);
```

## Responding to events
You can respond to events by declaring event handler functions inside your components:
function MyButton() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
Notice how onClick={handleClick} has no parentheses at the end! Do not call the event handler function: you only need to pass it down. React will call your event handler when the user clicks the button.

## updating the screen
Often, you’ll want your component to “remember” some information and display it. For example, maybe you want to count the number of times a button is clicked. To do this, add state to your component.

```js
import { useState } from 'react';

export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update together</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}

function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Clicked {count} times
    </button>
  );
}
```
You’ll get two things from useState: the current state (count), and the function that lets you update it (setCount). You can give them any names, but the convention is to write [something, setSomething].

The first time the button is displayed, count will be 0 because you passed 0 to useState(). When you want to change state, call setCount() and pass the new value to it. Clicking this button will increment the counter:

each button “remembers” its own count state and doesn’t affect other buttons.

To make both MyButton components display the same count and update together, you need to move the state from the individual buttons “upwards” to the closest component containing all of them.

Then, pass the state down from MyApp to each MyButton, together with the shared click handler. You can pass information to MyButton using the JSX curly braces, just like you previously did with built-in tags like <img>:

The information you pass down like this is called props. Now the MyApp component contains the count state and the handleClick event handler, and passes both of them down as props to each of the buttons.

Finally, we changeg MyButton to read the props you have passed from its parent component

When you click the button, the onClick handler fires. Each button’s onClick prop was set to the handleClick function inside MyApp, so the code inside of it runs. That code calls setCount(count + 1), incrementing the count state variable. The new count value is passed as a prop to each button, so they all show the new value. This is called “lifting state up”. By moving state up, you’ve shared it between components.

## Hooks
- Functions starting with use are called Hooks. 
- useState is a built-in Hook
- You can also write your own Hooks by combining the existing ones.

Hooks are more restrictive than other functions. You can only call Hooks at the top of your components (or other Hooks). If you want to use useState in a condition or a loop, extract a new component and put it there.

# Understanding `useState` in React

`useState` is a React Hook that allows you to add state to functional components. It's one of the most fundamental hooks in React and replaces the need for class components in many cases.

## Basic Usage

```jsx
import { useState } from 'react';

function Counter() {
  // Declare a state variable called "count" with initial value 0
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

## How it Works

1. `useState` returns an array with two elements:
   - The current state value
   - A function to update that state

2. The syntax `const [count, setCount] = useState(0)` uses array destructuring to assign these to variables.

## Key Characteristics

1. **Initial State**: The argument passed to `useState` is the initial state value (in the example above, `0`).

2. **State Updates**: When you call the state updater function (like `setCount`), React will:
   - Update the state value
   - Re-render the component with the new value

3. **Functional Updates**: When the new state depends on the previous state, you can pass a function to the updater:

```jsx
setCount(prevCount => prevCount + 1);
```

4. **Object State**: You can store objects in state:

```jsx
const [user, setUser] = useState({ name: 'John', age: 30 });

// Update with spread operator to maintain immutability
setUser(prevUser => ({ ...prevUser, age: 31 }));
```

## Important Notes

1. **Multiple State Variables**: You can use multiple `useState` calls in a single component:

```jsx
const [name, setName] = useState('Mary');
const [age, setAge] = useState(25);
```

2. **No Merging**: Unlike `this.setState` in class components, `useState` doesn't merge objects automatically.

3. **Initial State Computation**: If the initial state is expensive to compute, you can pass a function:

```jsx
const [state, setState] = useState(() => {
  const initialState = someExpensiveComputation();
  return initialState;
});
```

4. **State is Local**: State is local to the component instance. Each instance gets its own state.

`useState` is a powerful tool that makes functional components just as capable as class components when it comes to managing state.

# Understanding Props in React

Props (short for "properties") are a fundamental concept in React that allow you to pass data from parent components to child components. They are read-only and help make your components reusable and modular.

## Basic Usage of Props

```jsx
// Parent Component
function App() {
  return <Greeting name="Alice" message="Welcome!" />;
}

// Child Component
function Greeting(props) {
  return (
    <h1>
      Hello, {props.name}! {props.message}
    </h1>
  );
}
```

## Destructuring Props

A more modern and cleaner approach is to destructure props:

```jsx
function Greeting({ name, message }) {
  return (
    <h1>
      Hello, {name}! {message}
    </h1>
  );
}
```

## Key Characteristics of Props

1. **Unidirectional Data Flow**: Props are passed from parent to child (top-down)
2. **Read-Only**: Child components cannot modify their props
3. **Can Be Any JavaScript Value**: Strings, numbers, arrays, objects, functions, even other React elements

## Passing Different Types of Props

```jsx
<UserProfile
  name="John Doe"          // String
  age={30}                 // Number
  isAdmin={true}           // Boolean
  hobbies={['reading', 'hiking']}  // Array
  address={{               // Object
    street: '123 Main St',
    city: 'New York'
  }}
  onLogin={() => {}}       // Function
  avatar={<Avatar />}      // React Element
/>
```

## Special Props

1. **Children Prop**: Content between component tags is passed as `props.children`

```jsx
function Card({ children }) {
  return <div className="card">{children}</div>;
}

// Usage
<Card>
  <h2>Title</h2>
  <p>Content goes here</p>
</Card>
```

2. **Default Props**: You can define default values for props

```jsx
function Greeting({ name = 'Guest' }) {
  return <h1>Hello, {name}!</h1>;
}
```

Or using the component's `defaultProps` property:

```jsx
Greeting.defaultProps = {
  name: 'Guest',
  message: 'Welcome to our site'
};
```

## Prop Types (Type Checking)

For larger applications, you might want to validate prop types:

```jsx
import PropTypes from 'prop-types';

function User({ name, age }) {
  // component implementation
}

User.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number
};
```

## Props vs. State

| Props | State |
|-------|-------|
| Passed from parent component | Managed within component |
| Immutable (read-only) | Mutable (can be changed) |
| Used for configuration | Used for interactivity |
| Can be used in both functional and class components | Functional components use hooks, class components use `this.state` |

## Best Practices

1. Keep components small and focused by passing only necessary props
2. Avoid prop drilling (passing props through many layers) - consider context API or state management
3. Use descriptive prop names that indicate their purpose
4. Document your props using PropTypes or TypeScript
5. For complex components, consider using the spread operator:

```jsx
const userData = {
  name: 'Alice',
  age: 28,
  email: 'alice@example.com'
};

<UserProfile {...userData} />
```

Props are essential for creating reusable, composable components in React and understanding them is crucial for effective React development.

# Passing Functions via Props in React

Passing functions as props is a common pattern in React that allows child components to communicate with their parent components. This enables a unidirectional data flow while maintaining component reusability.

## Basic Example

```jsx
// Parent Component
function Parent() {
  const handleClick = () => {
    console.log('Button clicked in child!');
  };

  return <Child onClick={handleClick} />;
}

// Child Component
function Child({ onClick }) {
  return <button onClick={onClick}>Click Me</button>;
}
```

## Why Pass Functions as Props?

1. **Child-to-Parent Communication**: Allows children to trigger actions in parents
2. **Separation of Concerns**: Keeps business logic in parent components
3. **Reusability**: Same child component can be used with different behaviors

## Common Use Cases

### 1. Handling User Events

```jsx
function TodoList() {
  const handleComplete = (todoId) => {
    console.log(`Todo ${todoId} completed`);
  };

  return <TodoItem onComplete={handleComplete} />;
}

function TodoItem({ onComplete }) {
  return (
    <div>
      <span>Buy milk</span>
      <button onClick={() => onComplete(1)}>Complete</button>
    </div>
  );
}
```

### 2. Data Submission (Forms)

```jsx
function FormContainer() {
  const handleSubmit = (formData) => {
    console.log('Form submitted:', formData);
  };

  return <UserForm onSubmit={handleSubmit} />;
}

function UserForm({ onSubmit }) {
  const [name, setName] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    onSubmit({ name });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

### 3. State Updates

```jsx
function CounterParent() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Current count: {count}</p>
      <CounterControls 
        onIncrement={() => setCount(c => c + 1)}
        onDecrement={() => setCount(c => c - 1)}
      />
    </div>
  );
}

function CounterControls({ onIncrement, onDecrement }) {
  return (
    <div>
      <button onClick={onIncrement}>+</button>
      <button onClick={onDecrement}>-</button>
    </div>
  );
}
```

## Best Practices

1. **Descriptive Naming**: Use clear names like `onClick`, `onSubmit`, `onComplete`
2. **Avoid Inline Functions**: For performance, define functions outside render when possible
3. **Pass Parameters**: Use arrow functions to pass data back:

```jsx
<button onClick={() => onDelete(item.id)}>Delete</button>
```

4. **Type Checking**: Use PropTypes or TypeScript to validate function props:

```jsx
Button.propTypes = {
  onClick: PropTypes.func.isRequired
};
```

5. **Memoization**: For performance optimization with React.memo:

```jsx
const MemoizedChild = React.memo(Child);

function Parent() {
  const handleClick = useCallback(() => {
    console.log('Optimized click handler');
  }, []);

  return <MemoizedChild onClick={handleClick} />;
}
```

## Advanced Patterns

### 1. Render Props

```jsx
function MouseTracker({ render }) {
  const [position, setPosition] = useState({ x: 0, y: 0 });

  const handleMouseMove = (e) => {
    setPosition({ x: e.clientX, y: e.clientY });
  };

  return <div onMouseMove={handleMouseMove}>{render(position)}</div>;
}

// Usage
<MouseTracker render={({ x, y }) => (
  <p>Mouse position: {x}, {y}</p>
)} />
```

### 2. Higher-Order Components (HOCs)

```jsx
function withTimer(WrappedComponent) {
  return function(props) {
    const [time, setTime] = useState(new Date());

    useEffect(() => {
      const timer = setInterval(() => setTime(new Date()), 1000);
      return () => clearInterval(timer);
    }, []);

    return <WrappedComponent time={time} {...props} />;
  };
}

// Usage
const Clock = withTimer(function({ time }) {
  return <div>Current time: {time.toLocaleTimeString()}</div>;
});
```

Passing functions as props is a powerful pattern that enables component composition and maintains React's unidirectional data flow while allowing child components to trigger behavior in their parents.

# Ref
react documentation [intro](https://react.dev/learn)
