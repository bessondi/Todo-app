<template>
  <ul>
    <TodoItems
      v-for="(item, i) of tasks"
      v-bind:index="i + 1"
      v-bind:taskItem="item"
      v-bind:title="item.title"
      v-on:checkAsDone="checkAsDone"
      v-on:editTodo="editTodo(item, $event)"
      v-on:removeTask="removeTask"
    />
  </ul>
</template>


<script>
  import Todo from '@/components/Todo'

  export default {
    props: ['tasks'],

    components: {
      TodoItems: Todo
    },

    methods: {
      checkAsDone(id) {
        this.$emit('markListTask', id)
      },
      removeTask(id) {
        this.$emit('removeTaskFromList', id)
      },
      editTodo(item, $event) {
        this.$emit('editTaskFromList', item, $event)
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