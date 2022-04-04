---
title: React
---

# Create a React app

Chrome extension => React Dev Tools

## "Old"

~~~sh
# Instalar utilitário para criar base da aplicação
node install --global create-react-app

# Criar aplicação
create-react-app NOMEAPP
cd NOMEAPP

# Iniciar servidor de desenvolvimento
npm start

# Cria pacote para produção
npm run build

# Executa testes
npm test
~~~


## "New"

~~~sh
npx create-react-app react-task-tracker
cd react-task-tracker
npm start
~~~

# Files & folders

`src/App.js`:

~~~jsx
import React, { Component } from 'react';

class App extends Component {
  render() {
    return (
      <div>
        Hello World
      </div>
    );
  }
}
~~~

# App component & JSX


## Expressions in JSX

JSX:

    class -> className
    for -> htmlFor
  
## Creating a component

VScode extension: ES7 React/Reduct/GraphQL/React-Native snippets

    snippet: rafce -> React Arrow Function Component Export

### Function component 

~~~jsx
const App = () => {
  return (
    <h1>Hello</h1>
  )
}
~~~

### Class component

~~~jsx
import React 

class App extends React.Component {
  render() {
    return <h1>Hello</h1>
  }
}
~~~

## Component Props

~~~jsx
import PropTypes from 'prop-types'

const Header = (props) => {
  return (
    <h1>{props.title}</h1>
  )
}

Header.defaultProps = {
  title: 'Task Tracker',
}

export default Header
~~~

## PropTypes

~~~jsx
Header.propTypes = {
  title: PropTypes.string.isRequired,
}
~~~

## Styling

~~~html
<h1 style={{color: 'red'}}>props.title</h1>

<h1 style={alternativeStyle}>props.title</h1>
~~~

~~~jsx
const alternativeStyle = {
  color: 'red',
  backgroundColor: 'white',
};
~~~

## Button Component & Event

~~~jsx
import PropTypes from 'prop-types'

const Button = ({color, text, onClick}) => {
  return (
    <button style={{backgroundColor: color}} 
      className='btn' 
      onclick='onClick'>{text}</button>
  )
}

Button.defaultProps = {
  color: 'steelblue',
}

Button.propTypes = {
  text: PropTypes.string,
  color: PropTypes.string,
  onClick: PropTypes.func //.isRequired
}
~~~

## Component with children

~~~jsx
const Tasks = ({tasks}) => {
  return (
    <>
      {tasks.map((task) => { <h3 key={task.id}>{task.text}</h3> }}
    </>
  )
}
~~~

# State & useState Hook & Global state

At src/App.js:

~~~jsx
import { useState } from 'react'

const [tasks, setTasks] = useState([...])
~~~

# Icons with react-icons

~~~sh
npm install react-icons
~~~

~~~jsx
import { FaTimes } from 'react-icons'

  ...
  <h3>{task.text} <FaTimes style={{color: 'red', cursor: 'pointer'}}/></h3>
  ...
~~~

# Delete item on state

~~~jsx
const deleteTask = (id) => {
  setTasks(tasks.filter((task) => task.id !== id)
}
~~~

# Optional message if no items

~~~jsx
{ tasks.length > 0 ? <Tasks...> : 'No tasks' }
~~~

# JSON Server

~~~sh
npm i json-server
~~~

~~~json
"scripts": {
  "server": "json-server --watch db.json --port 5000"
}
~~~

# CRUD operations

## useEffect Hook & Fetch data from server

~~~jsx
useEffect(() => {
  getTasks();
}, [])

const fetchTasks = async () => {
  const res = await fetch(`http://localhost:5000/tasks`)
  const data = await res.json()
  return data
}
~~~

## Delete

~~~jsx
const deleteTask = async (id) => {
  await fetch(`http://localhost:5000/tasks/${id}`, {method: 'DELETE'})
}
~~~

## Create

~~~jsx
const addTask = async (task) => {
  const res = await fetch(`http://localhost:5000/tasks`, {
    method: 'POST',
    headers: {'Content-type': 'application/json'},
    body: JSON.stringify(task)
  })
  const data = await res.json()
  return data
}
~~~

## Update

~~~jsx
const fetchTask = async (id) => {
  const res = await fetch(`http://localhost:5000/tasks/${id}`)
  const data = await res.json()
  return data
}

const toggleReminder = async (task) => {
  const taskToToggle = await fetchTask(id)  
  const updTask = {...taskToToggle, reminder: !taskToToggle.reminder}
  const res = await fetch(`http://localhost:5000/tasks`, {
    method: 'PUT',
    headers: {'Content-type': 'application/json'},
    body: JSON.stringify(updTask)
  })
  const data = await res.json()
  
  setTasks(...)
}
~~~

# Routing, footer & about

## Routing

~~~sh
npm install react-router-dom
~~~

~~~jsx
import { BrowserRouter as Router, Route} from 'react-router-dom'

<Router>
  <Route path='/' exact render={(props) => (<> ... </>)}>
  <Route path='/about' component={About}>
</Router>
~~~

## Linking to routes

~~~jsx
import { Link } from 'react-router-dom'

Replace: <a href='...'>Example</a>
With: <Link to='...'>Example</Link>
~~~

## Keep track of current route

~~~jsx
import { useLocation } from 'react-router-dom'

const location = useLocation();

{ location.pathname === '/' && (<Button ... />) }
~~~
