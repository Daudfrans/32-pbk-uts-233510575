<script setup>
import { ref } from 'vue'

const tasks = ref([])
const input = ref('')

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
</script>

<template>
  <div>
    <input type="text" v-model="input" @keyup.enter="addTask" placeholder="Enter a task">
    <button @click="addTask">Add Task</button>
    <ul>
      <li v-for="task in tasks" :key="task.id">
        {{ task.text }}
      </li>
    </ul>
  </div>
</template>

<style scoped>

</style>
