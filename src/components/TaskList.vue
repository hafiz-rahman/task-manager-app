<script>
import { ref } from "vue";
import { state } from "../store/state";

export default {
  setup() {
    const showForm = ref(false);
    const newTask = ref("");

    // Show and hide add task form
    const toggleForm = () => {
      showForm.value = !showForm.value;
    };

    // Add new task
    const addTask = () => {
      if (newTask.value.trim().length >= 3) {
        state.tasks.push({ name: newTask.value.trim(), completed: false });
        newTask.value = "";
      } else {
        alert("Task name must be at least 3 characters.");
      }
    };

    // Delete task
    const deleteTask = (index) => {
      state.tasks.splice(index, 1);
    };

    // Task complete and undo
    const toggleComplete = (index) => {
      state.tasks[index].completed = !state.tasks[index].completed;
    };

    return {
      showForm,
      newTask,
      state,
      toggleForm,
      addTask,
      deleteTask,
      toggleComplete,
    };
  },
};
</script>

<template>
  <div>
    <!-- headline -->
    <button @click="toggleForm" class="toggle-button">
      {{ showForm ? "Hide Add Task Form" : "Show Add Task Form" }}
    </button>

    <!-- task add form-->
    <div v-if="showForm" class="form-container">
      <input
        type="text"
        v-model="newTask"
        placeholder="Enter task name"
        class="task-input"
      />
      <button @click="addTask" class="add-button">Add Task</button>
    </div>

    <!-- task list -->
    <div>
      <p v-if="!state.tasks.length" class="no-tasks">No tasks available.</p>
      <ul v-else class="task-list">
        <li
          v-for="(task, index) in state.tasks"
          :key="index"
          :class="{ completed: task.completed }"
        >
          {{ task.name }}
          <div>
          <button @click="toggleComplete(index)" class="complete-button">
            {{ task.completed ? "Undo" : "Complete" }}
          </button>
          <button @click="deleteTask(index)" class="delete-button">
            Delete
          </button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>

/* basic styling */
.toggle-button {
  display: block;
  margin: 20px auto;
  padding: 10px 20px;
  background-color: slateblue;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.form-container {
  text-align: center;
  margin: 20px 0;
}

.task-input {
  padding: 10px;
  width: 100%;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.add-button {
  padding: 10px 20px;
  margin-left: 10px;
  margin-top: 20px;
  text-align: center;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.no-tasks {
  text-align: center;
  color: #999;
  font-size: 1.2rem;
  margin: 20px 0;
}

.task-list {
  list-style-type: none;
  padding: 0;
  width: 100%;
  margin: 20px auto;
  align-items: center;
}

.task-list li {
  padding: 10px;
  margin: 10px 20px;
  border: 1px solid #ddd;
  border-radius: 5px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #f9f9f9;
}

.task-list li.completed {
  color: #ffffff;
  background-color: lightgreen;
  text-decoration: line-through;
  border: 1px solid red;
}

.complete-button {
  padding: 5px 10px;
  background-color: #2196f3;
  margin: 0 5px;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.delete-button {
  padding: 5px 10px;
  background-color: #f44336;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
</style>
