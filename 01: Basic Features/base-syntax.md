# 01: Base Features & Syntax

## Prerequisite

- [`Create-React-App`](https://github.com/facebook/create-react-app)

## Understanding Component Basics

```javascript
import React, { component } from "react";

class App extends Component {
  render() {
    return (
      <div className="App">
        <h1>Hi, I am React App</h1>
      </div>
    );
  }
}

export default App;
```

### What does this block of code do?

- It creates a javascript class `App` with the `class` keyword and then uses the `extends` keyword to inherit from the component class which is imported from the react library.

- This class has one method i.e. `render()` method. React will call this method to render html to the dom.

- Finally this class is exported.

Components are the **core building block of React apps**. Actually, React really is just a library for creating components in its core.

A typical React app therefore could be depicted as a **component tree** - having one root component `App` and potentially an infinite amount of nested child components.

Each component needs to return/render some **JSX** code - it defines which HTML code React should render to the real DOM in the end.

## Understanding JSX

JSX is just syntactic sugar for JavaScript, allowing users to write HTMLish code instead of nested `React.createElement()` methods.

```javascript
import React, { component } from "react";

class App extends Component {
  render() {
    // return (
    //   <div className="App">
    //     <h1>Hi, I am React App</h1>
    //   </div>
    // );
    return React.createElement(
      "div",
      { className: "App" },
      React.createElement("h1", null, "Hi, I am React app")
    );
  }
}

export default App;
```

The method takes at least three arguments:

1.  The first one is the element we want to render to the dom, which could be either a normal html element like a `div` or a custome component `Div`.
2.  The second argument is basically the configuration for this, a javascript object, but it is optional.
3.  The third argument here is any amount of child elements and which could be separated by commas.

The catch is if we want to render another element inside the div, what we have to do is call `React.createElement()` to create a new html element.

## JSX Restrictions

- `class` for example, which we would use in normal html to assign a css class cannot be used because it is a reserved word in javascript to create a new class. All these elements in this code snippet are managed or provided by the React library, because react is converting them behind the scenes.

- JSX expression must have only one root element. The best practice is to wrap everything into one root elemnt per component.

## Creating a Stateless component

**Stateless components** (also referred to as "presentational", "dumb" or "functional" components)

```javascript
import React from "react";

const person = () => {
  return <p> I am a person </p>;
};

export default person;
```

**Stateful components** (also referred to as "containers", "smart" or "class-based" components)

```javascript
import React, { component } from "react";
import "./App.css";
import Person from "./Person/Person";

class App extends Component {
  render() {
    return (
      <div className="App">
        <h1>Hi, I am React App</h1>
        <Person />
      </div>
    );
  }
}

export default App;
```

## Working with Props

```javascript
import React from "react";

const person = (props) => {
  return (
    <div>
      <p>
        I am {props.name} and I am {props.age} years old.
      </p>
      <p>{props.children}</p>
    </div>
  );
};

export default person;
```

`props` allow you to pass data from a parent (wrapping) component to a child (embedded) component.

## Understanding & using state

```javascript
import React, { component } from "react";
import "./App.css";
import Person from "./Person/Person";

class App extends Component {
  state = {
    persons: [
      { name: "Max", age: 28 },
      { name: "Manu", age: 29 },
    ],
  };

  render() {
    return (
      <div className="App">
        <h1>Hi, I am React App</h1>
        <Person
          name={this.state.persons[0].name}
          age={this.state.persons[0].age}
        />
      </div>
    );
  }
}

export default App;
```

Whilst props allow you to pass data down the component tree (and hence trigger an UI update), state is used to change the component from within.

Any changes to state will also trigger an UI update.

## setState()

```javascript
import React, { component } from "react";
import "./App.css";
import Person from "./Person/Person";

class App extends Component {
  state = {
    persons: [
      { name: "Max", age: 28 },
      { name: "Manu", age: 29 },
    ],
  };

  switchNameHandler = () => {
    this.setState({
      persons: [
        { name: "Youzhi", age: 28 },
        { name: "Manu", age: 30 },
      ],
    });
    // This method takes an object as an argument and will merge with the exisiting state, ensures React updates the DOM due to a change to state
  };

  render() {
    return (
      <div className="App">
        <h1>Hi, I am React App</h1>
        <button onclick={this.switchNameHandler}>Switch Name</button>
        // This is a reference to that property which holds a function
        <Person
          name={this.state.persons[0].name}
          age={this.state.persons[0].age}
        />
      </div>
    );
  }
}

export default App;
```

## React Hooks: useState()

```javascript
import React, { useState } from "react";
import "./App.css";
import Person from "./Person/Person";

const App = (props) => {
  const [personsState, setPersonsState] = useState({
    persons: [
      { name: "Max", age: 28 },
      { name: "Manu", age: 29 },
    ],
  });

  // setPersonsState does not merge whatever you pass to it with the old state, instead it replaces the old state with it.

  const [otherState, setOtherState] = useState("some other values");
  // In this case, use useState() multiple times if you have other states

  const switchNameHandler = () => {
    setPersonsState({
      persons: [
        { name: "Youzhi", age: 28 },
        { name: "Manu", age: 30 },
      ],
    });
  };

  return (
    <div className="App">
      <h1>Hi, I am React App</h1>
      <button onclick={switchNameHandler}>Switch Name</button>
      <Person
        name={personsState.persons[0].name}
        age={personsState.persons[0].age}
      />
    </div>
  );
};

export default App;
```

## Passing Method References Between Components

```javascript
class App extends Component {
  state = {
    persons: [{ name: "Youzhi", age: 28 }],
  };

  switchNameHandler = (newName) => {
    // console.log('Was clicked!');
    // DON'T DO THIS: this.state.persons[0].name = 'Maximilian';
    this.setState({
      persons: [
        { name: newName, age: 28 },
        { name: "Manu", age: 29 },
        { name: "Stephanie", age: 27 },
      ],
    });
  };

  nameChangedHandler = (event) => {
    this.setState({
      persons: [
        { name: "Max", age: 28 },
        { name: event.target.value, age: 29 },
        // target.value is the value the user entered
        { name: "Stephanie", age: 26 },
      ],
    });
  };

  render() {
    return (
      <div className="App">
        <Person
          name={this.state.persons[1].name}
          age={this.state.persons[1].age}
          click={this.switchNameHandler.bind(this, "Max!")}
          changed={this.nameChangedHandler}
        >
          My Hobbies: Racing
        </Person>
      </div>
    );
  }
}
```

```javascript
import React from "react";

const person = (props) => {
  return (
    <div>
      <p onClick={props.click}>
        I'm {props.name} and I am {props.age} years old!
      </p>
      <p>{props.children}</p>
      <input type="text" onChange={props.changed} value={props.name} />
      // two way binding and able to change name dynamically
    </div>
  );
};

export default person;
```

This clearly shows that React allows you to pass methods as props so that you can call a method which might change the state in another component which doesn't have direct access to the state.

```javascript
<button onClick={() => this.switchNameHandler("Youzhi!")}>Switch Name</button>
```

Alternatively, this shows another way of binding but an inefficient way of doing this. Usee bind()!
