# Introduction
In this part, we will familiarize ourselves with the React-library, which we will be using to write the code that runs in the browser. We will also look at some features of JavaScript that are important for understanding React.
***
## Introduction to React
We will be getting familiar with [React](https://react.dev/) library. The easiest way to get started by far is by using a tool called [Vite](https://vitejs.dev/) to create a simple React application.
```
# npm 6.x (outdated, but still used by some):
npm create vite@latest <app_name> --template react

# npm 7+, extra double-dash is needed:
npm create vite@latest <app_name -- --template react

cd <app_name>
# install the libraries
npm install

# to run the application
npm run dev
```
The console says that the application has started on localhost port 5173, i.e. the address http://localhost:5173. Vite starts the application by default on port 5173. If it is not free, Vite uses the next free port number.

The code of the application resides in the src folder. Mainly main.jsx which looks like
```
import ReactDOM from 'react-dom/client'

import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```
and App.jsx
```
const App = () => {
  return (
    <div>
      <p>Hello world</p>
    </div>
  )
}

export default App
```
The file App.jsx now defines a [React component](https://react.dev/learn/your-first-component) with the name App. The statement
`ReactDOM.createRoot(document.getElementById('root')).render(<App />)`
renders its contents into the div-element, defined in the file index.html, having the id value 'root'.

Technically the component is defined as a JavaScript function. The function is then assigned to a constant variable App. There are a few ways to define functions in JavaScript. Here we will use [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), which are described in a newer version of JavaScript known as [ECMAScript 6](http://es6-features.org/#Constants), also called ES6. Because the function consists of only a single expression we have used a shorthand.

#### JSX
It seems like React components are returning HTML markup. However, this is not the case. The layout of React components is mostly written using [JSX](https://react.dev/learn/writing-markup-with-jsx). Although JSX looks like HTML, we are dealing with a way to write JavaScript. Under the hood, JSX returned by React components is compiled into JavaScript. The compilation is handled by [Babel](https://babeljs.io/repl/). Projects created with create-react-app or vite are configured to compile automatically.

`It is also possible to write React as "pure JavaScript" without using JSX` but not recommended to do so.

JSX is much like HTML with the distinction that with JSX you can easily embed dynamic content by writing appropriate JavaScript within curly braces. JSX is "XML-like", which means that every tag needs to be closed.

#### Multiple components
Let's modify the file App.jsx as follows:

```
const Hello = () => {
  return (
    <div>
      <p>Hello world</p>
    </div>
  )
}

const App = () => {
  return (
    <div>
      <h1>Greetings</h1>
      <Hello />
	  <Hello />
      <Hello />
    </div>
  )
}
```
We have defined a new component Hello and used it inside the component App. Naturally, a component can be used multiple times.

Writing components with React is easy, and by combining components, even a more complex application can be kept fairly maintainable. Indeed, a core philosophy of React is composing applications from many specialized reusable components.

### Props: passing data to components
It is possible to pass data to components using so-called [props](https://react.dev/learn/passing-props-to-a-component).
```
const Hello = (props) => {
  return (
    <div>

      <p>Hello {props.name}</p>
    </div>
  )
}
const App = () => {
  return (
    <div>
      <h1>Greetings</h1>
      <Hello name='George' />
      <Hello name='Daisy' />
    </div>
  )
}
```
Now the function defining the component has a parameter props. As an argument, the parameter receives an object, which has fields corresponding to all the "props" the user of the component defines.
There can be an arbitrary number of props and their values can be "hard-coded" strings or the results of JavaScript expressions. If the value of the prop is achieved using JavaScript it must be wrapped with curly braces.

#### ESLint warnings
ESLint warnings such as [react/prop-types](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/prop-types.md) by adding them to the rules object in the file .eslintrc .cjs
as:
```
rules: {
     'react-refresh/only-export-components': [
       'warn',
       { allowConstantExport: true },
     ],
     'react/prop-types': 0
   },
```

### Some notes
 Keep in mind that First letter of React component names must be capitalized. If you try defining a component as follows:
 ```
const footer = () => {
  return (
    <div>
      greeting app created by <a href='https://github.com/mluukkai'>mluukkai</a>
    </div>
  )
}
const App = () => {
  return (
    <div>
      <h1>Greetings</h1>
      <Hello name='Maya' age={26 + 10} />

      <footer />
    </div>
  )
}
```
the page is not going to display the content defined within the Footer component, and instead React only creates an empty [footer](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/footer) element, i.e. the built-in HTML element instead of the custom React element of the same name.

React has been configured to generate quite clear error messages. Despite this, you should, at least in the beginning, advance in very small steps and make sure that every change works as desired.

The console should always be open. If the browser reports errors, it is not advisable to continue writing more code, instead try to understand the cause of the error and fix them or go back to the previous working state and advance again in very small steps.

The content of a React component (usually) needs to contain one root element.
```
const App = () => {
  return (
    <h1>Greetings</h1>
    <Hello name='Maya' age={26 + 10} />
    <Footer />
  )
}
```
will result in an error stating `JSX elements must be wrapped in an enclosing tag.`

Because the root element is stipulated, we have "extra" div elements in the DOM tree. This can be avoided by using fragments, i.e. by wrapping the elements to be returned by the component with an empty element:
```
const App = () => {
  const name = 'Peter'
  const age = 10

  return (
    <>
      <h1>Greetings</h1>
      <Hello name='Maya' age={26 + 10} />
      <Hello name={name} age={age} />
      <Footer />
    </>
  )
}
```
***
## JavaScript
The official name of the JavaScript standard is [ECMAScript](https://en.wikipedia.org/wiki/ECMAScript). At this moment, the latest version is the one released in June of 2023 with the name [ECMAScript®2023](https://www.ecma-international.org/ecma-262/), otherwise known as ES14.

Browsers do not yet support all of JavaScript's newest features. Due to this fact, a lot of code run in browsers has been transpiled from a newer version of JavaScript to an older, more compatible version.

Today, the most popular way to do transpiling is by using Babel. Transpilation is automatically configured in React applications created with Vite.

We use [Node.js](https://nodejs.org/en/) which is a JavaScript runtime environment based on Google's [Chrome V8](https://developers.google.com/v8/) JavaScript engine and works practically anywhere - from servers to mobile phones. 

JavaScript is sort of reminiscent, both in name and syntax, to Java. But when it comes to the core mechanism of the language they could not be more different. In certain circles, it has also been popular to attempt "simulating" Java features and design patterns in JavaScript. It is not recommended doing this, as the languages and respective ecosystems are ultimately very different.
#### Variables
```
const x = 1
let y = 5
```
[const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) does not define a variable but a constant for which the value can no longer be changed. On the other hand, [let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let) defines a normal variable.

A variable's data type can change during execution.
`y = 'sometext'`  is perfectly fine to use after defining y as shown above.

#### Arrays
```
const t = [1, -1, 3]

t.push(5)

t.forEach(value => {
  console.log(value)  // numbers 1, -1, 3, 5 are printed, each on its own line
})    
```
Notable in this example is the fact that the contents of the [array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) can be modified even though it is defined as a const. Because the array is an object, the variable always points to the same object. However, the content of the array changes as new items are added to it.

In the previous example, a new item was added to the array using the method [push](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push). When using React, techniques from functional programming are often used. One characteristic of the functional programming paradigm is the use of [immutable](https://en.wikipedia.org/wiki/Immutable_object) data structures. In React code, it is preferable to use the method [concat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat), which creates a new array with the added item. This ensures the original array remains unchanged.
`const t2 = t.concat(5)`  instead of `t.push(5)`

In JavaScript, unlike Java arrays do not have any datatype restrictions, as they are just a special kind of *Object*. They can contain a mix of datatypes in themselves. Though it is generally not recommended to do so.

Individual items of an array are easy to assign to variables with the help of the [destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment).
```
const t = [1, 2, 3, 4, 5]

const [first, second, ...rest] = t

console.log(first, second)  // 1, 2 is printed
console.log(rest)          // [3, 4, 5] is printed
```
Thanks to the assignment, the variables *first* and *second* will receive the first two integers of the array as their values. The remaining integers are "collected" into an array of their own which is then assigned to the variable *rest*.
#### Objects
There are a few different ways of defining objects in JavaScript. One very common method is using [object literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#Object_literals), which happens by listing its properties within braces:
```
const object1 = {
  name: 'Arto Hellas',
  age: 35,
  education: 'PhD',
}
```
The values of the properties can be of any type, like integers, strings, arrays, objects...

The properties of an object are referenced by using the "dot" notation, or by using brackets:

```
console.log(object1.name)         // Arto Hellas is printed
const fieldName = 'age' 
console.log(object1[fieldName])    // 35 is printedcopy
```
You can also add properties to an object on the fly by either using dot notation or brackets:

```
object1.address = 'Helsinki'
object1['secret number'] = 12341
```
The latter of the additions has to be done by using brackets because when using dot notation, *secret number* is not a valid property name because of the space character.

#### Functions
An example of simple arrow function:
```
const sum = (p1, p2) => {
  console.log(p1)
  console.log(p2)
  return p1 + p2
}
```
and called as:
```
const result = sum(1, 5)
console.log(result)
```
The other way to define the function is by using a [function expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/function). In this case, there is no need to give the function a name and the definition may reside among the rest of the code:

```
const average = function(a, b) {
  return (a + b) / 2
}

const result = average(2, 5)
// result is now 3.5
```
***
## Component state, event handlers
#### Component helper function

Let's expand our Hello component so that it guesses the year of birth of the person being greeted:

```
const Hello = (props) => {
  const bornYear = () => {
    const yearNow = new Date().getFullYear()
    return yearNow - props.age
  }

  return (
    <div>
      <p>
        Hello {props.name}, you are {props.age} years old
      </p>
      <p>So you were probably born in {bornYear()}</p>
    </div>
  )
}
```
The logic for guessing the year of birth is separated into a function of its own that is called when the component is rendered.

The person's age does not have to be passed as a parameter to the function, since it can directly access all props that are passed to the component.

If we examine our current code closely, we'll notice that the helper function is defined inside of another function that defines the behavior of our component. In Java programming, defining a function inside another one is complex and cumbersome, so not all that common. In JavaScript, however, defining functions within functions is a commonly-used technique.

#### Destructuring
we will take a look at a small but useful feature of the JavaScript language that was added in the ES6 specification, that allows us to [destructure](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) values from objects and arrays upon assignment.
```
const Hello = (props) => {
  const name = props.name
  const age = props.age
```
is same as:
```
const Hello = (props) => {
  const { name, age } = props
```
and same as:
```
const Hello = ({ name, age }) => {
```
The props that are passed to the component are now directly destructured into the variables, *name* and *age*.

#### Page re-rendering
We can get the component to re-render by calling the *render* method a second time or using this method:
```
let counter = 1

const refresh = () => {
  ReactDOM.createRoot(document.getElementById('root')).render(
    <App counter={counter} />
  )
}
setInterval(() => {
  refresh()
  counter += 1
}, 1000)
```
But making repeated calls to the render method is not the recommended way to re-render components. A better way of accomplishing this effect is done using ***Stateful component***

#### Stateful component
```
import { useState } from 'react'

const App = () => {
  const [ counter, setCounter ] = useState(0)
  setTimeout(
    () => setCounter(counter + 1),
    1000
  )

  return (
    <div>{counter}</div>
  )
}
```
The function call `const [ counter, setCounter ] = useState(0)` adds state to the component and renders it initialized with the value of zero. The function returns an array that contains two items. We assign the items to the variables counter and setCounter by using the destructuring assignment syntax. The variable setCounter is assigned a function that will be used to modify the state.

The application calls the [setTimeout](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout) function and passes it two parameters: a function to increment the counter state and a timeout of one second.

When the state modifying function setCounter is called, React re-renders the component which means that the function body of the component function gets re-executed. Every time the *setCounter* modifies the state it causes the component to re-render and will continue to do so as long as application is running.

#### Event Handling
A user's interaction with the different elements of a web page can cause a collection of various kinds of events to be triggered. Let's change the application so that increasing the counter happens when a user clicks a button, which is implemented with the [button](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button) element.

Button elements support so-called [mouse events](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent), of which [click](https://developer.mozilla.org/en-US/docs/Web/Events/click) is the most common event.
```
const App = () => {
  const [ counter, setCounter ] = useState(0)
  const handleClick = () => {
    setCounter(counter + 1)
  }

  return (
    <div>
      <div>{counter}</div>
      <button onClick={handleClick}>
        plus
      </button>
    </div>
  )
}
```
Now every click of the plus button causes the handleClick function to be called, meaning that every click event will increase the value of counter by *one* and component gets re-rendered.

`An event handler is a function or function reference and not actual function call.` and the above button declaration can also be done as:
```
<button onClick={() => setCounter(counter + 1)}> 
  plus
</button>
```
but not as:
```
<button onClick={setCounter(counter + 1)}> 
  plus
</button>
```
#### Passing state - to child components
Lets restructure the App.jsx as:
```
const App = () => {
  const [ counter, setCounter ] = useState(0)

  const increaseByOne = () => setCounter(counter + 1)
  const decreaseByOne = () => setCounter(counter - 1)
  const setToZero = () => setCounter(0)

  return (
    <div>
      <Display counter={counter}/>
      <Button
        onClick={increaseByOne}
        text='plus'
      />
      <Button
        onClick={setToZero}
        text='zero'
      />     
      <Button
        onClick={decreaseByOne}
        text='minus'
      />           
    </div>
  )
}
```
and Button component as:
```
const Button = (props) => {
  return (
    <button onClick={props.onClick}>
      {props.text}
    </button>
  )
}
```
and Display as:
```
const Display = ({ counter }) => <div>{counter}</div>
```
The event handler is passed to the Button component through the *handleClick* property. And button click in the *Button* component will change the value of *counter* as desired.

In React, it’s conventional to use onSomething names for props which represent events and handleSomething for the function definitions which handle those events.

**Its working is explained as:**
When the application starts, the code in App is executed. This code uses a useState hook to create the application state, setting an initial value of the variable counter. This component contains the Display component - which displays the counter's value, 0 - and three Button components. The buttons all have event handlers, which are used to change the state of the counter.

When one of the buttons is clicked, the event handler is executed. The event handler changes the state of the App component with the setCounter function. Calling a function that changes the state causes the component to rerender.
***
## A more complex state, debugging React apps
#### Complex state
In our previous example, the application state was simple as it was comprised of a single integer. What if our application requires more than one integer. Let's take this case:
```
const App = () => {
  const [left, setLeft] = useState(0)
  const [right, setRight] = useState(0)
```
which creates two pieces of state for the application named left and right that both get the initial value of 0 and our component gets access to the functions setLeft and setRight that it can use to update the two pieces of state.

We could implement the same functionality by saving the click count of both the left and right buttons into a single object:
```
const App = () => {
  const [clicks, setClicks] = useState({
    left: 0, right: 0
  })
```
Now the component only has a single piece of state and the event handlers have to take care of changing the entire application state.

The event handler looks a bit messy. When the left button is clicked, the following function is called:
```
const handleLeftClick = () => {
  const newClicks = { 
    left: clicks.left + 1, 
    right: clicks.right 
  }
  setClicks(newClicks)
}
```
We can define the new state object a bit more neatly by using the object spread syntax that was added to the language specification in the summer of 2018:
```
const handleLeftClick = () => {
  const newClicks = { 
    ...clicks, 
    left: clicks.left + 1 
  }
  setClicks(newClicks)
}
```
The syntax may seem a bit strange at first. In practice **{ ...clicks }** creates a new object that has copies of all of the properties of the *clicks* object. When we specify a particular property - e.g. left in { ...clicks, left: clicks.left + 1 }, the value of the left property in the new object will be *previous left* + 1.

The above handle function can be simplified as:
`const handleLeftClick = () => setClicks({ ...clicks, left: clicks.left + 1 })`

**It is forbidden in *React* to mutate state directly, since it can `result in unexpected side effects`. Changing state has to always be done by setting the state to a new object. If properties from the previous state object are not changed, they need to simply be copied, which is done by copying those properties into a new object and setting that as the new state.**

Storing all of the state in a single state object is a bad choice for this particular application; there's no apparent benefit and the resulting application is a lot more complex. In this case, storing the click counters into separate pieces of state is a far more suitable choice.

There are situations where it can be beneficial to store a piece of application state in a more complex data structure. [The official React documentation](https://react.dev/learn/choosing-the-state-structure) contains some helpful guidance on the topic.

#### Handling arrays
Let's add a piece of state to our application containing an array allClicks that remembers every click that has occurred in the application.

```
const App = () => {
  const [left, setLeft] = useState(0)
  const [right, setRight] = useState(0)
  const [allClicks, setAll] = useState([])

  const handleLeftClick = () => {
    setAll(allClicks.concat('L'))
    setLeft(left + 1)
  }
  const handleRightClick = () => {
    setAll(allClicks.concat('R'))
    setRight(right + 1)
  }

  return (
    <div>
      {left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {right}
      <p>{allClicks.join(' ')}</p>
    </div>
  )
}
```
The piece of state stored in allClicks is now set to be an array that contains all of the items of the previous state array plus the letter L. Adding the new item to the array is accomplished with the concat method, which does not mutate the existing array but rather returns a new copy of the array with the item added to it.

As mentioned previously, it's also possible in JavaScript to add items to an array with the push method. If we add the item by pushing it to the allClicks array and then updating the state, the application would still appear to work:
```
const handleLeftClick = () => {
  allClicks.push('L')
  setAll(allClicks)
  setLeft(left + 1)
}
```
**However, don't do this**. As mentioned previously, the state of React components like allClicks must not be mutated directly. Even if mutating state appears to work in some cases, it can lead to problems that are very hard to debug.

#### Update of the state is asynchronous
Let's expand the application so that it keeps track of the total number of button presses in the state total, whose value is always updated when the buttons are pressed:
```
const [total, setTotal] = useState(0)
const handleLeftClick = () => {
    setAll(allClicks.concat('L'))
    setLeft(left + 1)
    setTotal(left + right)
  }
```
This does not work and the The total number of button presses is consistently one less than the actual amount of presses. Even though a new value was set for left by calling setLeft(left + 1), the old value persists despite the update because state update in React happens ***asynchronously***, i.e. not immediately but "at some point" before the component is rendered again.

This problem can be fixed as:
```
const handleLeftClick = () => {
    setAll(allClicks.concat('L'))
    const updatedLeft = left + 1
    setLeft(updatedLeft)
    setTotal(updatedLeft + right) 
  }
```

#### [Conditional rendering](https://react.dev/learn/conditional-rendering)
```
const History = (props) => {
  if (props.allClicks.length === 0) {
    return (
      <div>
        the app is used by pressing the buttons
      </div>
    )
  }
  return (
    <div>
      button press history: {props.allClicks.join(' ')}
    </div>
  )
}

const App = () => {
  // ...
 
  return (
    <div>
      {left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {right}

      <History allClicks={allClicks} />
    </div>
  )
}
```
Now the behavior of the component depends on whether or not any buttons have been clicked. If not, meaning that the allClicks array is empty, the component renders a div element with `<div>the app is used by pressing the buttons</div>` And in all other cases, the component renders the clicking history.

#### Old React

In this course, we use the state hook to add state to our React components, which is part of the newer versions of React and is available from version 16.8.0 onwards. Before the addition of hooks, there was no way to add state to functional components. Components that required state had to be defined as class components, using the JavaScript class syntax.

#### Debugging React applications
A large part of a typical developer's time is spent on debugging and reading existing code. Every now and then we do get to write a line or two of new code, but a large part of our time is spent trying to figure out why something is broken or how something works. Good practices and tools for debugging are extremely important for this reason.

Old-school, print-based debugging is always a good idea. If the component is not working as intended, it's useful to start printing its variables out to the console. This will immediately reveal if, for instance, one of the attributes has been misspelled when using the component.

**When you use console.log for debugging, don't combine objects in a Java-like fashion by using the plus operator  `console.log('props value is ' + props)`.**

If you do that, you will end up with a rather uninformative log message `props value is [object Object]` 

Instead, separate the things you want to log to the console with a comma `console.log('props value is', props)` 

You can pause the execution of your application code in the Chrome developer console's debugger, by writing the command debugger anywhere in your code. The execution will pause once it arrives at a point where the debugger command gets executed.

By going to the Console tab, it is easy to inspect the current state of variables at this point in the ***scope-section***.

It is highly recommended to add the [React developer tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi) extension to Chrome. It adds a new Components tab to the developer tools. The new developer tools tab can be used to inspect the different React elements in the application, along with their state and props.

#### Rules of Hooks
There are a few limitations and rules we have to follow to ensure that our application uses hooks-based state functions correctly.

The **useState** function (as well as the **useEffect** function introduced later on in the course) must not be called from inside of a loop, a conditional expression, or any place that is not a function defining a component. This must be done to ensure that the hooks are always called in the same order, and if this isn't the case the application will behave erratically.

#### Do not define components within components
The application may still appear to work, but don't implement components within components. The method provides no benefits and leads to many unpleasant problems. The biggest problems are because React treats a component defined inside of another component as a new component in every render. This makes it impossible for React to optimize the component.

### Web programmers oath
Programming is hard, that is why I will use all the possible means to make it easier
- I will have my browser developer console open all the time
- I progress with small steps
- I will write lots of *console.log* statements to make sure I understand how the code behaves and to help pinpointing problems
- If my code does not work, I will not write more code. Instead I start deleting the code until it works or just return to a state when everything was still working


