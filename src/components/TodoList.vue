<template>
  <ul class="todoList__items">
    <TodoItems
      v-for="(item) of tasks"
      v-bind:taskItem="item"
      v-bind:title="item.title"
      v-on:editTodo="editTodo(item, $event)"
      v-on:checkAsDone="checkAsDone"
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


<style lang="scss" scoped>
  .todoList__items {
    display: flex;
    flex-direction: column;
    list-style: none;
    max-width: 400px;
    width: 100%;
    padding: 0;
    margin: 32px auto 40px;

    @media screen and (max-width: 768px) {
      margin: 32px auto;
    }
  }
</style>