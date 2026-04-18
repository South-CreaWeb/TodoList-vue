<script setup lang="ts">
import { ref, computed } from "vue"

type Tasks = {
  id: string,
  text: string,
  done: boolean
}

const task = ref<string>("")
const tasks = ref<Tasks[]>([])
const currentStatus = ref<"all" | "pending" | "done">("all")

function addTask() {

  if(!task.value.trim()) return

  const newTask = {
    id: crypto.randomUUID(),
    text: task.value,
    done: false
  }

  tasks.value.push(newTask)

  task.value = ""
}

function deleteTask(id: string) {
  tasks.value = tasks.value.filter(task => task.id !== id)
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

  return tasks.value
})

</script>


<template>
  <nav>
    <h1>Todo List with VueJS</h1>
  </nav>

  <section class="add_task">
    <h2>Add Task</h2>
    <div class="filter_task">
      <button @click="currentStatus = 'all'">All</button>
      <button @click="currentStatus = 'pending'">Pending</button>
      <button @click="currentStatus = 'done'">done</button>
    </div>
    <input v-model="task" type="text">
    <button @click="addTask">Add</button>
  </section>

  <section class="display_tasks">
    <ul>
      <li v-for="task in filteredTasks" :key="task.id" :class="{ disable: task.done }">
        <input v-model="task.done" type="checkbox">
        {{ task.text }}
        <button @click="deleteTask(task.id)">Delete</button>
      </li>
    </ul>
  </section>
</template>

<style>
.disable {
  text-decoration: line-through;
}
</style>
