<template>
  <form
    class="newTaskField"
    v-on:submit.prevent="newTask"
  >
<!--    <label for="newTask">Task:</label>-->
    <input
      id="newTask"
      class="newTaskField__titleField"
      type="text"
      placeholder="Add a new to-do..."
      v-model="title"
    >
    <button type="submit">Add</button>
  </form>
</template>


<script>
  export default {
    data() {
      return {
        title: ''
      }
    },

    props: {
      id: Number
    },

    methods: {
      newTask() {
        if (this.title.trim()) {

          const newTask = {
            id: this.id,
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


<style lang="scss" scoped>
  .newTaskField {
    &__titleField {
      width: 300px;
      padding: 4px 8px;
      margin: 20px 0 0 0;
      outline: none;
    }
  }
</style>