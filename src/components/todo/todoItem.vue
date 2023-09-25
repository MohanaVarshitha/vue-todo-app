<template>
  <input type="checkbox" @change="changed($event)"> 
  <span>{{ todo.who }}</span><br />{{ isSelected }}
  <button @click="editTodo()">Edit</button>
  <button @click="deleteTodo()" class="delete">Delete</button>
</template>

<script>
export default {
  name: 'TodoItem',
  props: {
    todo: {}
  },
  methods: {
    changed(e) {
      this.$emit('itemSelected', {
        ...this.todo,
        status: e.target.checked
      });
    },
    deleteTodo() {
      this.$emit('itemDeleted', this.todo);
    },
    editTodo() {
      this.$emit('itemEdited', this.todo);
    }
  }
}
</script>

<style>
.done {
  text-decoration: line-through;
}
.not-done {
  text-decoration: unset;
}
button {
  margin: 10px;
  border: 1px solid gray;
  padding: 10px;
}
.delete {
  background-color: red;
  border: unset;
}
input {
  margin-right: 20px;
}
</style>