# javascript1
## Aim:Create a fully functional To-Do List application using ES6 JavaScript. 
## Program:
### index.html:
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do List</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>Work To-Dos</h1>
        <p>Enter text into the input field to add items to your list.</p>
        <p>Click the item to mark it as complete.</p>
        <p>Click the "X" to remove the item from your list.</p>
        <form id="task-form">
            <input type="text" id="task-title" placeholder="New item...">
            <input type="text" id="task-desc" placeholder="Description...">
            <input type="date" id="task-date">
            <button type="submit">Add Task</button>
        </form>
        <ul id="task-list"></ul>
    </div>
    <script type="module" src="js.js"></script>
</body>
</html>
```
### Style.css
```
body {
  font-family: Arial, sans-serif;
  background-color: #c8d1c8;
  color: #dbc9c9;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
}

.container {
  background: #f8adad;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 0 10px rgba(247, 245, 245, 0.1);
  text-align: center;
  color: #333333;
  max-width: 500px;
  width: 100%;
}

form {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

form input, form button {
  padding: 10px;
  margin: 5px 0;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  background: #f9f9f9;
  margin: 5px 0;
  padding: 10px;
  border-radius: 4px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

li.completed {
  text-decoration: line-through;
  color: #999999;
}

li button {
  background: #d2cbcb;
  border: none;
  color: #ffffff;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
}

li button:hover {
  background: #ff2222;
}
```
### index.js:
```



import ToDoList from "./todolist.js";

const todoList = new ToDoList();

// Get DOM elements
const addTaskForm = document.getElementById("addTaskForm");
const taskList = document.getElementById("taskList");
const filterSelect = document.getElementById("filterSelect");

// Event listeners
addTaskForm.addEventListener("submit", (event) => {
  event.preventDefault();
  const title = document.getElementById("taskTitle").value;
  const description = document.getElementById("taskDescription").value;
  const dueDate = document.getElementById("taskDueDate").value;
  todoList.addTask(title, description, dueDate);
  renderTaskList();
  addTaskForm.reset();
});

filterSelect.addEventListener("change", () => {
  renderTaskList();
});

// Render the task list
function renderTaskList() {
  taskList.innerHTML = "";
  const tasksToDisplay = todoList.filterTasks(filterSelect.value);

  tasksToDisplay.forEach((task, index) => {
    const taskItem = document.createElement("li");
    taskItem.innerHTML = `
      <input type="checkbox" id="task-${index}" ${task.isCompleted ? "checked" : ""}>
      <label for="task-${index}">${task.title}</label>
      <button class="edit-btn">Edit</button>
      <button class="delete-btn">Delete</button>
    `;
    taskList.appendChild(taskItem);

    // Add event listeners for edit and delete buttons
    taskItem.querySelector(".edit-btn").addEventListener("click", () => {
      // Implement editing logic here
    });

    taskItem.querySelector(".delete-btn").addEventListener("click", () => {
      todoList.deleteTask(index);
      renderTaskList();
    });

    // Add event listener for checkbox
    taskItem.querySelector("input[type='checkbox']").addEventListener("change", () => {
      todoList.toggleTaskCompletion(index);
      renderTaskList();
    });
  });
}

// Initial rendering
renderTaskList();
```
### task.js:
```
export default class Task {
    constructor(title, description, dueDate, isCompleted = false) {
      this.title = title;
      this.description = description;
      this.dueDate = dueDate;
      this.isCompleted = isCompleted;
    }
  }
```
### todolist.js
```


// todoList.js
import Task from "./task.js";

export default class ToDoList {
  constructor() {
    this.tasks = this.loadTasksFromLocalStorage();
  }

  addTask(title, description, dueDate) {
    const newTask = new Task(title, description, dueDate);
    this.tasks.push(newTask);
    this.saveTasksToLocalStorage();
  }

  editTask(index, updatedTask) {
    this.tasks[index] = updatedTask;
    this.saveTasksToLocalStorage();
  }

  deleteTask(index) {
    this.tasks.splice(index, 1);
    this.saveTasksToLocalStorage();
  }

  toggleTaskCompletion(index) {
    this.tasks[index].isCompleted = !this.tasks[index].isCompleted;
    this.saveTasksToLocalStorage();
  }

  filterTasks(status) {
    if (status === "completed") {
      return this.tasks.filter(task => task.isCompleted);
    } else if (status === "incomplete") {
      return this.tasks.filter(task => !task.isCompleted);
    } else {
      return this.tasks;
    }
  }

  loadTasksFromLocalStorage() {
    const storedTasks = JSON.parse(localStorage.getItem("tasks"));
    if (storedTasks) {
      return storedTasks.map(taskData => new Task(
        taskData.title,
        taskData.description,
        taskData.dueDate,
        taskData.isCompleted
      ));
    } else {
      return [];
    }
  }

  saveTasksToLocalStorage() {
    localStorage.setItem("tasks", JSON.stringify(this.tasks));
  }
}
```
### Output:

![Screenshot (464)](https://github.com/KayyuruTharani/javascript1/assets/142209319/179a67a4-8353-445a-bce8-93ae497497cb)
