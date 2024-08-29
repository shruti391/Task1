# Task1
Creating a to-do list using HTML, CSS, and JavaScript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do List</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background: #e0f7fa; /* Light cyan background */
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            overflow: hidden;
        }

        .container {
            background: #ffffff; /* White background for container */
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 12px 24px rgba(0, 0, 0, 0.2);
            width: 360px;
            max-width: 90%;
            position: relative;
        }

        h1 {
            font-size: 32px;
            margin-bottom: 20px;
            color: #00796b; /* Deep teal for heading */
            text-align: center;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        form {
            display: flex;
            margin-bottom: 20px;
            gap: 10px;
        }

        input {
            flex: 1;
            padding: 12px;
            border: 2px solid #004d40; /* Dark teal border */
            border-radius: 8px;
            font-size: 16px;
            transition: border-color 0.3s, box-shadow 0.3s;
        }

        input:focus {
            border-color: #00796b;
            box-shadow: 0 0 5px rgba(0, 121, 107, 0.5); /* Teal glow on focus */
            outline: none;
        }

        button {
            padding: 12px 20px;
            background-color: #ff4081; /* Vibrant pink */
            color: #fff;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s, transform 0.2s;
        }

        button:hover {
            background-color: #f50057; /* Slightly darker pink */
            transform: scale(1.05);
        }

        ul {
            list-style: none;
            padding: 0;
            margin: 0;
        }

        li {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px;
            border-bottom: 1px solid #b2dfdb; /* Light teal border */
            border-radius: 8px;
            margin-bottom: 10px;
            background: #ffffff; /* White background for list items */
            transition: background-color 0.3s, transform 0.2s;
        }

        li:hover {
            background-color: #e0f2f1; /* Light cyan background on hover */
            transform: scale(1.02);
        }

        .task-text {
            flex: 1;
            font-size: 16px;
            color: #004d40; /* Dark teal text color */
        }

        .edit, .delete {
            background: none;
            border: none;
            font-size: 18px;
            cursor: pointer;
            transition: color 0.3s, transform 0.2s;
        }

        .edit {
            color: #ffab00; /* Vibrant orange for edit button */
        }

        .edit:hover {
            color: #ff6f00; /* Darker orange on hover */
            transform: scale(1.1);
        }

        .delete {
            color: #d32f2f; /* Vibrant red for delete button */
        }

        .delete:hover {
            color: #b71c1c; /* Darker red on hover */
            transform: scale(1.1);
        }

        /* Animation for task addition */
        .task-enter {
            opacity: 0;
            transform: translateY(-20px);
        }

        .task-enter-active {
            opacity: 1;
            transform: translateY(0);
            transition: opacity 0.3s, transform 0.3s;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>To-Do List</h1>
        <form id="task-form">
            <input type="text" id="task-input" placeholder="Enter a new task" required>
            <button type="submit">Add Task</button>
        </form>
        <ul id="task-list"></ul>
    </div>
    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const form = document.getElementById('task-form');
            const taskInput = document.getElementById('task-input');
            const taskList = document.getElementById('task-list');

            // Load tasks from localStorage
            loadTasks();

            form.addEventListener('submit', (e) => {
                e.preventDefault();
                const taskText = taskInput.value.trim();
                if (taskText) {
                    addTask(taskText);
                    taskInput.value = '';
                }
            });

            taskList.addEventListener('click', (e) => {
                if (e.target.classList.contains('edit')) {
                    editTask(e.target.closest('li'));
                } else if (e.target.classList.contains('delete')) {
                    deleteTask(e.target.closest('li'));
                }
            });

            function loadTasks() {
                const tasks = JSON.parse(localStorage.getItem('tasks')) || [];
                tasks.forEach(task => {
                    const li = createTaskElement(task.text, task.id);
                    taskList.appendChild(li);
                    // Trigger the enter animation
                    setTimeout(() => li.classList.add('task-enter-active'), 10);
                });
            }

            function saveTasks() {
                const tasks = Array.from(taskList.children).map(li => ({
                    text: li.querySelector('.task-text').textContent,
                    id: li.dataset.id
                }));
                localStorage.setItem('tasks', JSON.stringify(tasks));
            }

            function addTask(text) {
                const id = Date.now();
                const li = createTaskElement(text, id);
                li.classList.add('task-enter');
                taskList.appendChild(li);
                // Trigger the enter animation
                setTimeout(() => li.classList.add('task-enter-active'), 10);
                saveTasks();
            }

            function createTaskElement(text, id) {
                const li = document.createElement('li');
                li.dataset.id = id;
                li.innerHTML = `
                    <span class="task-text">${text}</span>
                    <div>
                        <button class="edit">✎</button>
                        <button class="delete">✘</button>
                    </div>
                `;
                return li;
            }

            function editTask(li) {
                const newText = prompt('Edit task:', li.querySelector('.task-text').textContent);
                if (newText) {
                    li.querySelector('.task-text').textContent = newText;
                    saveTasks();
                }
            }

            function deleteTask(li) {
                if (confirm('Are you sure you want to delete this task?')) {
                    li.classList.add('task-enter');
                    setTimeout(() => {
                        li.remove();
                        saveTasks();
                    }, 300);
                }
            }
        });
    </script>
</body>
</html>
