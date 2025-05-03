<script setup>
import { ref, computed } from 'vue'

const tasks = ref([])
const input = ref('')
const filter = ref('all')

const addTask = () => {
  if (input.value.trim() !== '') {
    tasks.value.push({
      id: Date.now(),
      text: input.value,
      completed: false
    })
    input.value = ''
  }
}

const removeTask = (task) => {
  tasks.value = tasks.value.filter(t => t.id !== task.id)
}

const toggleTask = (task) => {
  task.completed == !task.completed
}

const filteredTasks = computed(() => {
  if (filter.value === 'all') {
    return tasks.value
  } else if (filter.value === 'completed') {
    return tasks.value.filter(task => task.completed)
  } else {
    return tasks.value.filter(task => !task.completed)
  }
})

</script>

<template>
  <div>
    <input type="text" v-model="input" @keyup.enter="addTask" placeholder="Enter a task">
    <button @click="addTask">Add Task</button>

    <div>
      <button @click="filter = 'all'">All</button>
      <button @click="filter = 'completed'">Completed</button>
      <button @click="filter = 'active'">Active</button>
    </div>

    <ul>
      <li v-for="task in filteredTasks" :key="task.id">
        <input type="checkbox" v-model="task.completed" @change="toggleTask(task)">
        {{ task.text }}
        <button @click="removeTask(task)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<style scoped></style>
