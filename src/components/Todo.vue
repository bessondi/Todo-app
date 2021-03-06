<template>
  <li class="todo"
      v-bind:class='{ done: taskItem.completed }'
  >
    <input
      class="todo__checkbox"
      type="checkbox"
      v-bind:checked="taskItem.completed"
      v-on:change="$emit('checkAsDone', taskItem.id, $event)"
    >

    <span
      class="todo__title"
      v-bind:class='{ done: taskItem.completed }'
      v-on:click="startEditing()"
      v-if="!isEditing"
    >
        {{ taskItem.title | uppercase }}
    </span>

    <form
      class="todo__editField"
      v-else v-on:submit.prevent="finishEditing()"
    >
      <input
        class="todo__textArea"
        type="text"
        v-model="newTodoTitle"
        v-on:blur="finishEditing()"
        ref="newTodo"
      />
    </form>

    <button
      class="todo__removeTodoBtn"
      v-on:click="$emit('removeTask', taskItem.id, $event)"
    >&times;</button>
  </li>
</template>


<script>
  export default {
    data() {
      return {
        isEditing: false,
        newTodoTitle: ""
      };
    },
    props: {
      taskItem: {
        id: Date.now(),
        type: Object,
        required: true
      },
      index: Number
    },
    methods: {
      startEditing() {
        if (this.isEditing) {
          this.finishEditing()
        } else {

          this.newTodoTitle = this.taskItem.title
          this.isEditing = true
          // возвращает td к обычному состоянию
          this.$nextTick(() => this.$refs.newTodo.focus())
        }
      },
      finishEditing() {
        this.isEditing = false;
        this.$emit("editTodo", this.newTodoTitle)
      }
    },
    filters: {
      uppercase(value) {
        // return value.toUpperCase()
        return value.charAt(0).toUpperCase() + value.slice(1)
      }
    }
  }
</script>


<style lang="scss" scoped>
  @import '../scss/colors';


  .todo {
    display: grid;
    grid-template-columns: 30px minmax(50px, 1fr) 30px;
    grid-template-rows: minmax(40px, auto);
    align-items: center;
    justify-items: center;
    grid-template-areas: "checkbox title remove";
    position: relative;
    padding: 16px;
    margin: 8px 0;
    border-radius: 15px;
    border: 2px solid $accentBlueColor;
    color: $textColor;
    box-shadow: 0 5px 5px rgba(93, 141, 238, .2);
    transition: 0.5s;

    &:hover {
      box-shadow: 0 5px 5px rgba(173, 95, 230, .4);
      transition: 0.5s;
    }

    &__checkbox {
      grid-area: checkbox;
      width: 25px;
      height: 25px;
      border-radius: 50%;
      margin: 0;
      border: 2px solid $checkboxBorder;
      outline: none;
      cursor: pointer;
      transition: 0.3s;
      appearance: none;
      -webkit-appearance: none;
      -moz-appearance: none;


      &:hover {
        background-color: $checkboxHover;
        border: 2px solid $checkboxChecked;
        transition: 0.3s;
      }

      &:checked {
        background-color: $accentLightGrayColor;
        border: 2px solid $checkboxHover;
        transition: 0.3s;

        &:hover {
          background-color: $doneTextColor;
          transition: 0.3s;
        }
      }
    }

    &__title {
      grid-area: title;
      max-width: 270px;
      padding: 8px 12px;
      text-align: left;
      justify-self: flex-start;
      cursor: pointer;
      font-weight: bold;
      overflow-wrap: break-word;
      word-wrap: break-word;
      word-break: break-word;
      hyphens: auto;
      -ms-hyphens: auto;
      -moz-hyphens: auto;
      -webkit-hyphens: auto;
    }

    &__editField {
      margin: 0 auto;
      max-width: 270px;
      width: 90%;
    }

    &__textArea {
      max-width: 270px;
      width: 100%;
      height: 100%;
      padding: 8px;
      font-size: 1.2rem;
      outline: none;
      border: none;
      transition: 0.3s;
      font-family: 'Avenir', sans-serif;
      font-weight: bold;
    }

    &__removeTodoBtn {
      grid-area: remove;
      position: absolute;
      width: 30px;
      height: 30px;
      border-radius: 10px;
      font-size: 1.3rem;
      display: inline-block;
      border: none;
      outline: none;
      color: #f1f1f1;
      background-color: $accentPinkColor;
      cursor: pointer;
      transition: 0.2s;
      line-height: 5px;
      box-sizing: border-box;

      &:hover {
        background-color: $accentCrimsonColor;
        transition: 0.2s;
      }
    }

    .done {
      text-decoration: line-through;
      color: $doneTextColor;
      transition: 0.3s;
    }

    &.done {
      border-color: $accentLightGrayColor;
      background-color: $checkboxChecked;
    }

    @media screen and (max-width: 768px) {
      margin: 8px 20px;
    }
  }
</style>