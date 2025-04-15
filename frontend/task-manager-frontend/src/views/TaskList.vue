<template>
  <div class="max-w-2xl mx-auto mt-10 p-4 bg-white shadow rounded">
    <h1 class="text-2xl font-bold mb-4">Task Manager</h1>

    <form @submit.prevent="submitTask">
      <input v-model="form.title" placeholder="Title" class="input" />
      <input v-model="form.description" placeholder="Description" class="input" />
      <button class="bg-blue-500 text-white px-4 py-2 mt-2 rounded">
        {{ form.id ? 'Update' : 'Create' }} Task
      </button>
    </form>

    <ul class="mt-6">
      <li v-for="task in tasks" :key="task.id" class="border-b py-2 flex justify-between items-center">
        <div>
          <h3 :class="{ 'line-through': task.completed }" class="font-semibold">{{ task.title }}</h3>
          <p class="text-sm">{{ task.description }}</p>
        </div>
        <div class="flex gap-2">
          <button @click="editTask(task)" class="text-blue-500">Edit</button>
          <button @click="deleteTask(task.id)" class="text-red-500">Delete</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const tasks = ref([])
const form = ref({ title: '', description: '', completed: false })

const fetchTasks = async () => {
  const res = await axios.get('http://localhost:5000/api/tasks')
  tasks.value = res.data
}

const submitTask = async () => {
  if (form.value.id) {
    await axios.put(`http://localhost:5000/api/tasks/${form.value.id}`, form.value)
  } else {
    await axios.post('http://localhost:5000/api/tasks', form.value)
  }
  form.value = { title: '', description: '', completed: false }
  fetchTasks()
}

const editTask = (task) => {
  form.value = { ...task }
}

const deleteTask = async (id) => {
  await axios.delete(`http://localhost:5000/api/tasks/${id}`)
  fetchTasks()
}

onMounted(fetchTasks)
</script>

<style>
.input {
  display: block;
  width: 100%;
  padding: 0.5rem;
  margin-top: 0.25rem;
  margin-bottom: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 6px;
}
</style>
