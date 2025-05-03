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
  task.completed = !task.completed
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
  <div class="min-h-screen bg-gray-900 text-white p-4 flex items-center justify-center">
    <div class="bg-gray-800 w-full h-100 max-w-5xl rounded-lg shadow-lg p-6 flex flex-col md:flex-row gap-6">

      <div class="md:w-1/2 flex flex-col gap-4">
        <h2 class="text-xl font-bold text-blue-400">📋 Tambah & Filter Tugas</h2>

        <input type="text" v-model="input" @keyup.enter="addTask" placeholder="Tugas baru..."
          class="px-4 py-2 rounded bg-gray-700 border border-gray-600 placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-blue-500" />

        <button @click="addTask" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded transition">
          Tambahkan
        </button>

        <div class="flex gap-3 mt-4">
          <button @click="filter = 'all'"
            :class="filter === 'all' ? 'bg-blue-600 text-white' : 'bg-gray-700 text-gray-300'"
            class="px-3 py-1 rounded-full font-semibold hover:bg-blue-500">
            Semua
          </button>
          <button @click="filter = 'completed'"
            :class="filter === 'completed' ? 'bg-blue-600 text-white' : 'bg-gray-700 text-gray-300'"
            class="px-3 py-1 rounded-full font-semibold hover:bg-blue-500">
            Selesai
          </button>
          <button @click="filter = 'active'"
            :class="filter === 'active' ? 'bg-blue-600 text-white' : 'bg-gray-700 text-gray-300'"
            class="px-3 py-1 rounded-full font-semibold hover:bg-blue-500">
            Aktif
          </button>
        </div>
      </div>

      <div class="md:w-1/2 flex flex-col">
        <h2 class="text-xl font-bold text-blue-400 mb-4">🗂️ Daftar Tugas</h2>
        <div class="overflow-y-auto h-[400px] pr-2">
          <ul class="space-y-2">
            <li v-for="task in filteredTasks" :key="task.id"
              class="flex items-center justify-between bg-gray-700 rounded px-4 py-2">
              <div class="flex items-center gap-3">
                <input type="checkbox" v-model="task.completed" @change="toggleTask(task)"
                  class="accent-blue-600 w-4 h-4" />
                <span :class="{ 'line-through text-gray-400': task.completed }">{{ task.text }}</span>
              </div>
              <button @click="removeTask(task)" class="text-red-400 hover:text-red-600 font-semibold">
                ✕
              </button>
            </li>
          </ul>
        </div>
      </div>

    </div>
  </div>
</template>

<style scoped>
</style>
