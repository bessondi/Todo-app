<template>
  <form v-on:submit.prevent="newTask">
    <input class="taskInput" type="text" v-model="title">
    <button type="submit">ADD</button>
  </form>
</template>


<script>
  export default {
    data() {
      return {
        title: ''
      }
    },

    methods: {
      newTask() {
        if (this.title.trim()) {

          const newTask = {
            id: Date.now(),
            title: this.title,
            completed: false
          }

          this.$emit('addTask', newTask)
          this.title = ''
        }
      }
    }
  }
</script>


<style scoped>
  .taskInput {
    width: 300px;
    padding: 4px 8px;
    outline: none;
  }
</style>