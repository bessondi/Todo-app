<template>
  <li class="todo">
    <input
      class="todo__checkbox"
      type="checkbox"
      v-bind:checked="taskItem.completed"
      v-on:change="$emit('checkAsDone', taskItem.id)"
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
      v-on:click="$emit('removeTask', taskItem.id)"
    >&times;
    </button>
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
          this.finishEditing();
        } else {

          this.newTodoTitle = this.taskItem.title;
          this.isEditing = true;
          // возвращает td к обычному состоянию
          this.$nextTick(() => this.$refs.newTodo.focus());
        }
      },
      finishEditing() {
        this.isEditing = false;
        this.$emit("editTodo", this.newTodoTitle);
      }
    },
    filters: {
      uppercase(value) {
        return value.toUpperCase()
      }
    }
  }
</script>


<style lang="scss" scoped>
  .todo {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 16px;
    margin: 8px 0;
    background: #002494;
    color: white;
    border-radius: 10px;

    &__checkbox {
      &:hover{
        cursor: pointer;
      }
    }

    &__title {
      width: 100%;
      margin: 0;
      padding: 8px 0;
      margin: 0 16px;
      text-align: left;

      &:hover{
        cursor: pointer;
      }
    }

    &__editField {
    }
    &__textArea {
    }

    &__removeTodoBtn {
      width: 30px;
      height: 25px;
      border-radius: 50%;
      border: none;
      outline: none;
      background-color: #fff;
    }

    .done {
      text-decoration: line-through;
      color: #c1c1c1;
    }
  }
</style>