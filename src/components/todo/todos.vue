<template>
  <div class="main">
    <h2>To-Do List</h2>
    <h4>What needs to be done?</h4>
    <div>
      <input type="text" :value="newTodo" @keyup="todoChanged($event)" v-on:keyup.enter="addTodo">
      <br />
      <button @click="addTodo()">{{this.isEditMode ? 'UPDATE': 'ADD'}}</button>
      <h2 v-if="this.todos.length > 0">{{ this.todos.filter(t => t.status).length }} out of {{ this.todos.length }} items completed</h2>
    </div>
    <div v-for="item in todos" :key="item.id">
      <todoItem
        :todo="item"
        @itemSelected="itemSelected($event)"
        @itemDeleted="itemDeleted($event)"
        @itemEdited="itemEdited($event)"></todoItem>
    </div>
  </div>
</template>

<script>
import todoItem from './todoItem.vue';
export default {
  name: 'TodoApp',
  data() {
    return {
      todos: [],
      newTodo: '',
      isEditMode: false,
      editTodo: {}
    }
  },
  components: {
    todoItem
  },
  methods: {
    todoChanged(e) {
      this.newTodo = e.target.value;
    },
    addTodo() {
      if (this.isEditMode) {
        const index = this.todos.findIndex(t => t.id === this.editTodo.id);
        this.todos[index].who = this.newTodo;
        this.isEditMode = false;
      } else {
        this.newTodo && this.todos.push({
          id: this.todos.length,
          who: this.newTodo,
          status: false
        });
      }
      this.newTodo = '';
    },
    itemSelected(todo) {
      const index = this.todos.findIndex(t => t.id === todo.id);
      this.todos[index].status = todo.status;
    },
    itemDeleted(todo) {
      const index = this.todos.findIndex(t => t.id === todo.id);
      this.todos = [...this.todos.slice(0, index), ...this.todos.slice(index + 1)]
    },
    itemEdited(todo) {
      this.newTodo = todo.who;
      this.isEditMode = true;
      this.editTodo = todo;
    }
  }
}
</script>

<style scoped>
.main {
  background-color: white;
  width: 30%;
  margin: auto;
  border-radius: 10px;
  padding: 20px;
}
</style>