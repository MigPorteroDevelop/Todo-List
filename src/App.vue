<script>
import Navbar from "./components/navbar.vue";
import Alert from "./components/alert.vue";
import AddTodoForm from "./components/addTodoForm.vue";
import Todo from "./components/todo.vue"
import Modal from "./components/modal.vue";
import Btn from "./components/btn.vue";

export default {
  components: {
    Alert,
    Navbar,
    AddTodoForm,
    Todo,
    Modal,
    Btn
},
  data() {
    return {
      todoTitle: "",
      todos: [],
      showAlert: false,
      editTodo: false
    };
  },
  methods: {
    addTodo(title) {
      if (title === "") {
        this.showAlert = true;
        return;
      }
      this.todos.push({
        title,
        id: Math.floor(Math.random() * 1000),
      });

    },
    deleteTodo(id) {
      this.todos = this.todos.filter((todo) => todo.id !== id);
    },
  },
};
</script>

<template>
  <div>
    <Navbar />
    <main class="container">
      <Modal :show="editTodo">
        <template v-slot:header>
          <h2>Edit Todo</h2>
        </template>
        <template v-slot:content>
          <form class="edit-todo-form">
            <label>Todo Title</label>
            <input type="text" placeholder="Edit the content">
          </form>
        </template>
        <template v-slot:footer>
          <Btn>Submit</Btn>
          <Btn type="danger" @click="editTodo = false">Close</Btn>
        </template>
      </Modal>
      <Alert 
      message="Todo title is required" 
      :show="showAlert" 
      @close="showAlert = false" />

      <section>
        <AddTodoForm 
        @submit="addTodo" 
        />
      </section>

      <section>
        <Todo 
        v-for="todo in todos" 
        :key="todo.id" 
        :title="todo.title" 
        @delete="deleteTodo(todo.id)" 
        @edit="editTodo = true"
        />
      </section>
    </main>
  </div>
</template>

<style scoped>
.edit-todo-form{
  height: 6rem;  
}
.edit-todo-form > input{
  width: 100%;
  height: 3rem;
  margin-top: 0.5rem;
  border: 1px solid var(--accent-color); 
}
</style>