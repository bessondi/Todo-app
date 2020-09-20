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
  @import '../scss/colors';


  .newTaskForm {

    &__title {
      max-width: 400px;
      width: 100%;
      padding: 8px 0;
      margin: 40px 0 0;
      font-size: 1.2rem;
      outline: none;
      border: none;
      border-bottom: 1px solid $accentLightGrayColor;
      cursor: pointer;
      transition: border 0.3s;
      border-radius: 0;

      &:focus {
        border-bottom: 2px solid $accentBlueColor;
        transition: border-bottom 0.3s;
      }

      &::placeholder {
        color: $accentLightGrayColor;
      }

      @media screen and (max-width: 768px) {
        max-width: 360px;
        width: 85%;
      }
    }

    // &__addTodoBtn {
      // width: 50px;
      // height: 50px;
      // border-radius: 50%;
      // border: none;
      // outline: none;
      // color: #f1f1f1;
      // background-color: $accentPinkColor;
      // cursor: pointer;
      // transition: 0.3s;
    // }
  }
</style>