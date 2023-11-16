<script lang="ts">
export default {
  data() {
    return {
      task: '',
      list: [''],
    }
  },
  methods: {
    addToList() {
        this.list.push(this.task)
        this.task = ''
    },
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
    <ul>
      <li v-for="(item, index) in list" :key="index">{{ item }}</li>
    </ul>
  </div>
</template>
