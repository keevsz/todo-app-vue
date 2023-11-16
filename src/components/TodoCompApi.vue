<script lang="ts">
import { ref } from 'vue'

interface Task {
  id: number
  task: string
  completed: boolean
}

export default {
  setup() {
    const task = ref('')

    const list = ref<Task[]>(
      window.localStorage.getItem('list')
        ? JSON.parse(window.localStorage.getItem('list') || '')
        : []
    )

    const addToList = () => {
      list.value.push({
        id: list.value.length + 1,
        task: task.value,
        completed: false,
      })
      window.localStorage.setItem('list', JSON.stringify(list.value))
      task.value = ''
    }

    const removeFromList = (id: number) => {
      const newArray = list.value.filter((task) => {
        return task.id !== id
      })
      window.localStorage.setItem('list', JSON.stringify(newArray))
      list.value = newArray
    }

    const changeStatus = (id: number) => {
      const newArray = list.value.map((task) => {
        return {
          ...task,
          completed: task.id === id ? !task.completed : task.completed,
        }
      })
      window.localStorage.setItem('list', JSON.stringify(newArray))
      list.value = newArray
    }

    return {
      task,
      list,
      addToList,
      removeFromList,
      changeStatus,
    }
  },
}
</script>

<template>
  <div class="flex flex-col justify-center items-center gap-5">
    <div class="text-3xl">To-Do List</div>
    <div class="flex flex-row gap-3">
      <input type="text" v-model="task" class="text-black" />
      <button @click="addToList">Add</button>
    </div>
    <ul class="flex flex-col justify-center items-center max-w-[calc(400px)]">
      <li v-for="(item, index) in list" :key="index">
        {{ item.task }}
        <button
          @click="changeStatus(item.id)"
          class="{{ item.completed ? 'text-green-400' : 'text-red-400' }}"
        >
          {{ item.completed ? '✅' : '❌' }}
        </button>
        <button @click="removeFromList(item.id)" class="text-red-400">
          | Borrar
        </button>
      </li>
    </ul>
  </div>
</template>
