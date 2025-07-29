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

# Ref
react documentation [intro](https://react.dev/learn)
