# 1stOOP-smallProject

Pour créer un projet simple en JavaScript utilisant l'architecture VMC (View-Model-Controller) avec des principes de programmation orientée objet (OOP), voici un exemple de projet : une petite application de gestion de tâches.

### Étape 1 : Définir les fichiers

1. `index.html` : pour le balisage de l'interface utilisateur.
2. `app.js` : pour le code JavaScript, y compris les classes OOP, le modèle, le contrôleur, et la vue.

### Étape 2 : Créer le fichier `index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Task Manager</title>
    <style>
      body {
        font-family: Arial, sans-serif;
      }
      .task-list {
        list-style-type: none;
        padding: 0;
      }
      .task-item {
        padding: 5px;
        margin: 5px 0;
        border: 1px solid #ccc;
      }
      .completed {
        text-decoration: line-through;
      }
    </style>
  </head>
  <body>
    <h1>Task Manager</h1>
    <input type="text" id="taskInput" placeholder="New task" />
    <button id="addTaskBtn">Add Task</button>
    <ul id="taskList" class="task-list"></ul>

    <script src="app.js"></script>
  </body>
</html>
```

### Étape 3 : Créer le fichier `app.js`

```javascript
// Model
class Task {
  constructor(name) {
    this.name = name;
    this.completed = false;
  }

  toggleCompletion() {
    this.completed = !this.completed;
  }
}

// Controller
class TaskController {
  constructor(view) {
    this.view = view;
    this.tasks = [];
  }

  addTask(name) {
    if (name.trim() !== "") {
      const task = new Task(name);
      this.tasks.push(task);
      this.view.renderTasks(this.tasks);
    }
  }

  toggleTaskCompletion(index) {
    this.tasks[index].toggleCompletion();
    this.view.renderTasks(this.tasks);
  }
}

// View
class TaskView {
  constructor() {
    this.taskInput = document.getElementById("taskInput");
    this.addTaskBtn = document.getElementById("addTaskBtn");
    this.taskList = document.getElementById("taskList");
  }

  bindEvents(controller) {
    this.addTaskBtn.addEventListener("click", () => {
      const taskName = this.taskInput.value;
      controller.addTask(taskName);
      this.taskInput.value = "";
    });

    this.taskList.addEventListener("click", (e) => {
      if (e.target && e.target.classList.contains("task-item")) {
        const index = Array.from(this.taskList.children).indexOf(e.target);
        controller.toggleTaskCompletion(index);
      }
    });
  }

  renderTasks(tasks) {
    this.taskList.innerHTML = "";
    tasks.forEach((task, index) => {
      const taskItem = document.createElement("li");
      taskItem.textContent = task.name;
      taskItem.classList.add("task-item");
      if (task.completed) taskItem.classList.add("completed");
      this.taskList.appendChild(taskItem);
    });
  }
}

// Initialize MVC components
const view = new TaskView();
const controller = new TaskController(view);
view.bindEvents(controller);
```

### Explication

1. **Model (`Task`)** : Représente les données. Ici, il a un nom et un état de complétion.
2. **Controller (`TaskController`)** : Gère la logique métier. Il ajoute des tâches et bascule leur état de complétion.
3. **View (`TaskView`)** : Gère l'affichage. Elle rend les tâches à l'écran et lie les événements aux actions du contrôleur.

Avec ce code, vous avez une petite application de gestion de tâches en utilisant JavaScript avec une architecture simple VMC. Vous pouvez l'étendre et l'améliorer à mesure que vous vous familiarisez avec les concepts.
