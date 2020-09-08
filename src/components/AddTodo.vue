<template>
  <form
    class="newTaskForm"
    v-on:submit.prevent="newTask"
  >
    <input
      id="newTask"
      class="newTaskForm__title"
      type="text"
      placeholder="Add a new todo.."
      v-model="title"
    >
<!--    <button type="submit" class="newTaskForm__addTodoBtn">+</button>-->
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
  $accentRedColor: #E63946;
  $accentLightBlueColor: #A8DADC;
  $checkboxHover: #d0d0d0;


  .newTaskForm {

    &__title {
      max-width: 400px;
      width: 100%;
      padding: 8px 0;
      margin: 40px 0 0;
      font-size: 1.4rem;
      outline: none;
      border: none;
      border-bottom: 1px solid $accentLightBlueColor;
      cursor: pointer;
      transition: border 0.3s;

      &:focus {
        border-bottom: 2px solid $accentRedColor;
        transition: border-bottom 0.3s;
      }

      &::placeholder {
        color: $accentLightBlueColor;
      }

      @media screen and (max-width: 768px) {
        width: 80%;
      }
    }

    &__addTodoBtn {
      width: 50px;
      height: 50px;
      border-radius: 10px;
      border: none;
      outline: none;
      color: #f1f1f1;
      background-color: $accentRedColor;
      cursor: pointer;
      transition: 0.3s;
    }
  }
</style>