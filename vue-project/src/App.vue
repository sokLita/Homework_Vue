<!-- <template>
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
</style> -->


<template>
  <div class="min-h-screen bg-slate-100 font-sans">

    <!-- Header -->
    <header class="bg-indigo-700 text-white sticky top-0 z-50 shadow-md">
      <div class="max-w-7xl mx-auto px-6 h-14 flex items-center justify-between">
        <div class="flex items-center gap-2 font-bold text-base">
          <span class="text-xl"></span>
          <span>Vue JS Props Practice Lab</span>
        </div>
        <span class="text-sm opacity-80">Lita Sok</span>
      </div>
    </header>

    <main class="max-w-7xl mx-auto px-6 py-7">

      <!-- Progress Banner -->
      <div class="bg-white rounded-xl shadow-sm p-5 mb-6">
        <div class="flex justify-between text-sm text-slate-500 mb-2">
          <span>Course Progress</span>
          <strong class="text-slate-700">{{ progress }}%</strong>
        </div>
        <div class="h-2 bg-slate-200 rounded-full overflow-hidden">
          <div
            class="h-full bg-gradient-to-r from-indigo-600 to-amber-400 rounded-full transition-all duration-500"
            :style="{ width: progress + '%' }"
          ></div>
        </div>
      </div>

      <!-- Stat Cards -->
      <section class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-7">
        <StartCard
          v-for="stat in stats"
          :key="stat.title"
          :title="stat.title"
          :value="stat.value"
          :label="stat.label"
          :status="stat.status"
        />
      </section>

      <!-- Content Grid -->
      <div class="grid grid-cols-1 lg:grid-cols-[1fr_300px] gap-6 items-start">

        <!-- Left -->
        <div>
          <h2 class="text-sm font-bold uppercase tracking-wide text-slate-500 mb-3">
            Upcoming Assignments
          </h2>
          <div class="flex flex-col gap-3">
            <AssignmentCard
              v-for="a in assignments"
              :key="a.title"
              :title="a.title"
              :website="a.website"
              :status="a.status"
              :dueDate="a.dueDate"
              @submit="handleSubmit"
            />
          </div>

          <h2 class="text-sm font-bold uppercase tracking-wide text-slate-500 mt-7 mb-3">
            Action Items
          </h2>
          <div class="flex flex-col gap-3">
            <ActionItem
              v-for="item in actionItems"
              :key="item.task"
              :task="item.task"
              :type="item.type"
              :link="item.link"
              @done="handleDone"
            />
          </div>
        </div>

        <!-- Right -->
        <div class="flex flex-col gap-4">
          <StudentProfile
            :avatar="studentProfile.avatar"
            :name="studentProfile.name"
            :bio="studentProfile.bio"
            :plans="studentProfile.plans"
          />
          <Constraints
            :courseName="constraints.courseName"
            :instructor="constraints.instructor"
            :progress="constraints.progress"
          />
        </div>

      </div>
    </main>

    <!-- Toast -->
    <transition
      enter-active-class="transition duration-300"
      enter-from-class="opacity-0 translate-y-3"
      leave-active-class="transition duration-300"
      leave-to-class="opacity-0 translate-y-3"
    >
      <div
        v-if="toast.visible"
        class="fixed bottom-7 right-7 px-5 py-3 rounded-lg text-sm font-semibold text-white shadow-lg z-50"
        :class="toast.type === 'success' ? 'bg-green-600' : 'bg-indigo-600'"
      >
        {{ toast.message }}
      </div>
    </transition>

  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import StartCard        from './components/StartCard.vue'
import AssignmentCard  from './components/AssignmentCard.vue'
import ActionItem      from './components/ActionItem.vue'
import StudentProfile  from './components/StudentProfile.vue'
import Constraints     from './components/Constraints.vue'

const stats = ref([
  { title: 'Lessons Completed', value: 12, label: '/ 20', status: 'good' },
  { title: 'Exercises Done',    value: 8,  label: '/ 15', status: 'good' },
  { title: 'Quizzes Submitted', value: 3,  label: '/ 5',  status: 'pending' },
  { title: 'Attendance Rate',   value: 92, label: '%',    status: 'excellent' },
])

const progress = computed(() => {
  const done = stats.value.reduce((s, x) => s + x.value, 0)
  return Math.round((done / (20 + 15 + 5 + 100)) * 100)
})

const assignments = ref([
  { title: 'Composition API Quiz',  website: 'Submission Form', status: 'pending',   dueDate: '3 days ago' },
  { title: 'Subcomponent Tutorial', website: 'Tutorial Site',   status: 'completed', dueDate: '1 week ago' },
])



const studentProfile = ref({
  avatar: 'https://img.favpng.com/3/11/24/3d-woman-avatar-stylized-cartoon-woman-avatar-with-glasses-g0FutwYY_t.jpg',
  name: 'Alex Morgan',
  bio: 'Frontend Developer',
  plans: ['Composition API Quiz', 'Subcomponent Tutorial', 'Style A'],
})

const constraints = ref({
  courseName: 'Completed Transactions This',
  instructor: 'Prof. Jason',
  progress: 57,
})

const toast = ref({ visible: false, message: '', type: 'success' })

function showToast(msg, type = 'success') {
  toast.value = { visible: true, message: msg, type }
  setTimeout(() => { toast.value.visible = false }, 3000)
}

function handleSubmit(title) { showToast(`Submitted: ${title}`, 'success') }
function handleDone(task)    { showToast(`Marked done: ${task}`, 'info') }
</script>
