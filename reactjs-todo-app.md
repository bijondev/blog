# Here's a basic example of a to-do app using React:

```
import React, { useState } from "react";

function TodoApp() {
  const [todos, setTodos] = useState([
    { text: "Learn about React", completed: false },
    { text: "Meet friend for lunch", completed: false },
    { text: "Build a to-do app in React", completed: false }
  ]);

  function handleTodoClick(index) {
    const newTodos = [...todos];
    newTodos[index].completed = !newTodos[index].completed;
    setTodos(newTodos);
  }

  return (
    <div className="todo-app">
      <h1>To-Do List</h1>
      <ul>
        {todos.map((todo, index) => (
          <li
            key={index}
            style={{ textDecoration: todo.completed ? "line-through" : "none" }}
            onClick={() => handleTodoClick(index)}
          >
            {todo.text}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default TodoApp;
```

This example uses the useState hook to manage the state of the todos array, and the map function to render each to-do item as a list item. The handleTodoClick function is used to toggle the completed property of a to-do item when it's clicked.

