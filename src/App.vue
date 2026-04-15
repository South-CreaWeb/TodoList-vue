<script setup lang="ts">
import { ref } from "vue"

type Tasks = {
  id: string,
  text: string,
  done: boolean
}

const task = ref<string>("")
const tasks = ref<Tasks[]>([])

function addTask() {

  const newTask = {
    id: crypto.randomUUID(),
    text: task.value,
    done: false
  }

  tasks.value.push(newTask)

  task.value = ""
}

</script>


<template>
  <nav>
    <h1>Todo List with VueJS</h1>
  </nav>

  <section class="add_task">
    <h2>Add Task</h2>
    <div class="filter_task">
      <button>All</button>
      <button>Pending</button>
      <button>done</button>
    </div>
    <input v-model="task" type="text">
    <button @click="addTask">Add</button>
  </section>

  <section class="display_tasks">
    <ul>
      <li v-for="task in tasks" :key="task.id">
        {{ task.text }}
      </li>
    </ul>
  </section>
</template>

<style>

</style>
