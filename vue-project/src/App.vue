<template>
  <div>
    <h1>Task Manager</h1>

    <div class="title">
      <input
        type="text"
        placeholder="Please input your task"
        v-model="taskTitle"
      />

      <select v-model="taskPriority">
        <option value="Low">Low</option>
        <option value="Medium">Medium</option>
        <option value="High">High</option>
      </select>

      <button @click="addTask">Add Task</button>
    </div>

    <p>
      {{ doneCount }} of {{ tasks.length }} done
    </p>

    <ul v-if="tasks.length > 0"> 
      <li v-for="task in tasks" :key="task.id">
        <input type="checkbox" v-model="task.done" />

        <span :class="{ completed: task.done }">
          {{ task.name }}
        </span>

        <span
          :class="{
            low: task.priority === 'Low',
            medium: task.priority === 'Medium',
            high: task.priority === 'High'
          }"
        >
          ({{ task.priority }})
        </span>

        <button @click="deleteTask(task.id)">
          Delete
        </button>
      </li>
    </ul>

    <p v-else>No tasks available</p>
  </div>
</template>


<script setup>
import { ref, computed } from "vue";


const taskTitle = ref("");
const taskPriority = ref("Low");

const tasks = ref([
  {
    id: 1,
    name: "Learn Vue",
    priority: "Medium",
    done: false
  },
  {
    id: 2,
    name: "Review Vue",
    priority: "High",
    done: false
  },
  {
    id: 3,
    name: "Doing Homework",
    priority: "Low",
    done: true
  },
  {
    id: 4,
    name: "Reading English",
    priority: "High",
    done: false
  },
  {
    id: 5,
    name: "Listening English",
    priority: "Low",
    done: false
  }
]);

const doneCount = computed(() => {
  return tasks.value.filter(task => task.done).length;
});

const addTask = () => {
  if (!taskTitle.value.trim()) return;

  const task = {
    id: Date.now(),
    name: taskTitle.value,
    priority: taskPriority.value,
    done: false
  };

  tasks.value.push(task);

  taskTitle.value = "";
  taskPriority.value = "Low";
};

const deleteTask = (id) => {
  tasks.value = tasks.value.filter(task => task.id !== id);
};
</script>

<style scoped>
.container {
  max-width: 500px;
  margin: 40px auto;
  font-family: Arial;
}

h1 {
  text-align: center;
}

.form {
  display: flex;
  gap: 8px;
  margin-bottom: 15px;
}

input, select {
  padding: 6px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 6px 10px;
  border: none;
  background: blue;
  color: white;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background: darkblue;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  padding: 8px 0;
  border-bottom: 1px solid #eee;
}

.left {
  display: flex;
  gap: 8px;
  align-items: center;
}

.done {
  text-decoration: line-through;
  color: gray;
}

.priority {
  font-size: 12px;
  padding: 2px 6px;
  border-radius: 4px;
  color: white;
}

.low {
  color: red;
}

.medium {
  color: orange;
}

.high {
  color: green;
}

.delete {
  color: red;
}

.delete:hover {
  background: darkred;
}

.status {
  margin-bottom: 10px;
  color: #555;
}

.empty {
  text-align: center;
  color: gray;
}
.title{
  display: flex;
  gap: 24px;
}
</style>