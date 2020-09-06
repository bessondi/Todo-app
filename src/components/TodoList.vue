<template>
  <ul>
    <TodoItems
            v-for="(item, i) of tasks"
            v-bind:index="i + 1"
            v-bind:taskItem="item"
            v-on:checkAsDone="checkAsDone"
            v-on:removeTask="removeTask"
    />
  </ul>
</template>


<script>
  import TodoItems from '@/components/TodoItems'

  export default {
    props: ['tasks'],

    components: {
      TodoItems
    },

    methods: {
      checkAsDone(id) {
        this.$emit('checkTaskInList', id)
      },
      removeTask(id) {
        this.$emit('removeTaskFromList', id)
      }
    }
  }
</script>


<style scoped>
  ul {
    display: flex;
    flex-direction: column;
    list-style: none;
    width: 350px;
    padding: 0;
    margin: 0 auto;
  }
</style>