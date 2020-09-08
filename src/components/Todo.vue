<template>
  <li>
    <input
      type="checkbox"
      v-bind:checked="taskItem.completed"
      v-on:change="$emit('checkAsDone', taskItem.id)"
    >

    <span
      class="task"
      v-bind:class='{ done: taskItem.completed }'
      v-on:click="startEditing()"
      v-if="!isEditing"
    >
        {{ taskItem.title | uppercase }}
    </span>

    <form v-else @submit.prevent="finishEditing()">
      <input
        type="text"
        v-model="newTodoTitle"
        v-on:blur="finishEditing()"
        ref="newTodo"
      />
    </form>

    <button
      class="btn"
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


<style scoped>
  li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 15px;
    margin: 8px 0;
    background: #002494;
    color: white;
    border-radius: 10px;
  }

  .task {
    width: 100%;
    margin: 0;
    padding: 5px 20px;
    text-align: left;
  }

  .btn {
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
</style>