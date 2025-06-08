# Task Manager with Basic Caching

This project demonstrates a simple task management application with a basic caching mechanism implemented using `localStorage`. The goal is to improve perceived performance by reducing redundant "API calls" (simulated in this case) and loading data faster.

## Features

* **Task Management:** Add and remove tasks from a list.
* **Basic Caching:**
    * On application load, it attempts to fetch tasks from `localStorage` first.
    * If tasks are found in `localStorage`, they are used immediately for faster rendering.
    * If no cached tasks are found, or a refresh is forced, tasks are "fetched" from a simulated server and then saved to `localStorage`.
    * When tasks are added or removed, both the "server" (simulated `localStorage` for server data) and the local cache are updated to maintain consistency.
* **Manual Cache Refresh:** A "Refresh Cache" button is provided to explicitly force a data fetch from the "server," bypassing the cache.

## How Caching is Implemented

The caching mechanism follows these steps:

1.  **On App Load (`loadTasks` function):**
    * The application first checks `localStorage` for a key named `"tasks"`.
    * If `localStorage.getItem("tasks")` returns a value, it means cached data exists. This data is then `JSON.parse()`d and immediately used to render the task list. This provides a very fast initial load.
    * If no cached data is found, or if the `forceRefresh` parameter is `true` (e.g., from the "Refresh Cache" button), the `fetchTasksFromServer()` function is called.
    * Once the "server" (simulated) responds with the task list, this data is `JSON.stringify()`d and saved to `localStorage` using `localStorage.setItem("tasks", JSON.stringify(tasks))` for future use. The tasks are then rendered.

2.  **Updating Cache on Data Changes:**
    * When a new task is added via the form (`addTaskForm` submission), the `addTaskToServer()` function is called. This function simulates sending the new task to a backend and returns the *updated* list of tasks from the "server."
    * Crucially, after the "server" update, `localStorage.setItem("tasks", JSON.stringify(updatedTasks))` is called to update the local cache with the latest, most accurate data from the "server."
    * Similarly, when a task is removed, `removeTaskFromServer()` is called, and the local cache is updated with the returned `updatedTasks` from the "server."

## Simulated Server

For this example, the "server" is also simulated using a separate `localStorage` key (`"server_tasks_mock"`). This allows the application to behave as if it's interacting with a backend, but without requiring an actual server setup. In a real-world application, `fetchTasksFromServer`, `addTaskToServer`, and `removeTaskFromServer` would make actual `fetch` API calls to a backend server.

## Getting Started

To run this application:

1.  **Save the files:** Create a folder named `task-app-with-caching`.
2.  Inside this folder, save the `index.html`, `style.css`, and `script.js` files as provided.
3.  **Open `index.html`:** Open the `index.html` file in your web browser.

You can observe the caching behavior by:

* Opening the browser's developer console (F12) and navigating to the "Application" tab -> "Local Storage". You'll see the "tasks" key.
* Reloading the page: The first load might show "Fetching tasks from server...", subsequent loads should show "Using cached tasks." (unless the cache is empty or you force a refresh).
* Adding/removing tasks and observing how the `localStorage` value for "tasks" updates.
* Clicking the "Refresh Cache" button to force a new fetch from the "server."