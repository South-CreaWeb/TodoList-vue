<script setup lang="ts">
import { ref, computed, onMounted } from "vue"

type Tasks = {
  id: string,
  text: string,
  done: boolean
}

const task = ref<string>("")
const tasks = ref<Tasks[]>([])
const currentStatus = ref<"all" | "pending" | "done">("all")

function initLocalStorage() {

  const initStorage = localStorage.getItem("tasks")

  if(initStorage) {
    try {
      tasks.value = JSON.parse(initStorage)
    } catch {
      tasks.value = []
    }
  }
}

onMounted(() => {
  initLocalStorage()
})

function addTask() {

  if(!task.value.trim()) return

  const newTask = {
    id: crypto.randomUUID(),
    text: task.value,
    done: false
  }

  tasks.value.push(newTask)

  localStorage.setItem("tasks", JSON.stringify(tasks.value))

  task.value = ""

}

function deleteTask(id: string) {

  tasks.value = tasks.value.filter(task => task.id !== id)

  localStorage.setItem("tasks", JSON.stringify(tasks.value))

}

function saveTask() {

  localStorage.setItem("tasks", JSON.stringify(tasks.value))

}

const filteredTasks = computed(() => {

  if(currentStatus.value === "all") {
    return tasks.value
  }

  if(currentStatus.value === "pending") {
    return tasks.value.filter(task => !task.done)
  }

  if(currentStatus.value === "done") {
    return tasks.value.filter(task => task.done)
  }

  localStorage.setItem("tasks", JSON.stringify(tasks.value))

  return tasks.value
})

</script>


<template>
  <nav>
    <h1>Todo List with VueJS</h1>
  </nav>

  <section class="new_task">
    <div class="add_task">
      <h2>Add Task</h2>
      <input v-model="task" type="text">
      <button @click="addTask">Add</button>
    </div>
    <div class="filter_task">
      <h2>Filter</h2>
      <button :class="{ active: currentStatus === 'all' }" class="filter_left" @click="currentStatus = 'all'">All</button>
      <button :class="{ active: currentStatus === 'pending' }"  @click="currentStatus = 'pending'">Pending</button>
      <button :class="{ active: currentStatus === 'done' }" class="filter_right" @click="currentStatus = 'done'">done</button>
    </div>
  </section>

  <section class="display_tasks">
    <ul>
      <li v-for="task in filteredTasks" :key="task.id">
        <div class="message_task">
          <input v-model="task.done" type="checkbox" @change="saveTask">
          <span :class="{ disable: task.done }">{{ task.text }}</span>
        </div>
        <button @click="deleteTask(task.id)">Delete</button>
      </li>
    </ul>
  </section>
</template>

<style>
/** GLOBAL */
body {
  font-family: 'Monserrat' ,sans-serif;
  background-color: #2f2f2f;
  font-size: 1rem;
}

h1 {
  font-size: 3.5rem;
}

h2 {
  font-size: 2.3rem;
}

h1, h2 {
  color: #42b883;
}

nav {
  display: flex;
  justify-content: center;
  margin-top: 20px;
  margin-bottom: 50px;
}

/** NEW TASK __ ADD TASK*/
.new_task {
  display: flex;
  justify-content: space-around;
  margin: 0px 20px;
}

.add_task input {
  border: none;
  padding: 5px 10px;
  border-radius: 10px 0 0 10px;
}

.add_task input:focus {
  outline: none;
}

.add_task button {
  border: none;
  border-radius: 0 10px 10px 0;
}

.add_task button:hover {
  background-color: #55eba7;
}
/** DISPLAY TASKS */
.display_tasks {
  display: flex;
  justify-content: center;
  margin-top: 50px;
}

.display_tasks ul {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
  justify-content: center;
  gap: 15px;
}

.display_tasks li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 50%;
  padding: 25px 25px;
  background-color: #1D293D;
  border: 1px solid #1D293D;
  border-radius: 10px;
}

.display_tasks button {
  border: 1px solid #dc3545;
  border-radius: 10px;
  background-color: #dc3545;
}

.display_tasks button:hover {
  background-color: #ff4e60;
}

.message_task input {
  margin-right: 15px;
}

.message_task span {
  color: white;
  font-size: 1.5rem;
}

/** FILTERED PART */
.active {
  background-color: #256b4c;
}

.disable {
  text-decoration: line-through;
}

.filter_task button {
  border: none;
}

.filter_task button:hover {
  background: #55eba7;
}

.filter_task .filter_left {
  border-radius: 10px 0 0 10px;
}

.filter_task .filter_right {
  border-radius: 0 10px 10px 0;
}

/** BUTTONS */
button {
  padding: 5px 10px;
  color: white;
  background-color: #42b883;
}
</style>
