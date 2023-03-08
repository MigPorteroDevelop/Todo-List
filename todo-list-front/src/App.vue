<script>
import Alert from "./components/Alert.vue";
import Navbar from "./components/Navbar.vue";
import AddTodoForm from "./components/AddTodoForm.vue";
import Todo from "./components/Todo.vue";
import Modal from "./components/Modal.vue";
import Btn from "./components/Btn.vue";
import axios from "axios";
import Spinner from "./components/spinner.vue"

export default {
  components: {
    Alert,
    Navbar,
    AddTodoForm,
    Todo,
    Modal,
    Btn,
    Spinner
  },

  data() {
    return {
      todoTitle: "",
      todos: [],
      alert:{
        show: false,
        message: ""
      },
      isLoading: false,
      isPostingTodo: false,
      editTodoForm: {
        show: false,
        todo: {
          id: 0,
          title: "",
        },
      },
    };
  },
  created() {
    this.fetchTodos();
  },

  methods: {
    async fetchTodos() {
      this.isLoading = true;
      try {
        const res = await axios.get('/api/todos');
        this.todos = res.data;
      }catch(e){
        this.showAlert("There was a problem loading the list", "info");
      }
      this.isLoading = false;
    },
    showAlert(message, type = ""){
      this.alert.show = true;
      this.alert.message = message;
      this.alert.type = type;
    },

    async addTodo(title) {
      if (title === "") {
        this.showAlert("Todo title is required", "danger");
        return;
      }

      this.isPostingTodo = true;
      const res = await axios.post('/api/todos', {
        title
      });
      this.isPostingTodo = false;

      console.log(res.data);
      this.todos.push(res.data);
    },
    showEditTodoForm(todo) {
      this.editTodoForm.show = true;
      this.editTodoForm.todo = { ...todo };
    },
    updateTodo() {
      // find the todo to be updated
      const todo = this.todos.find(
        (todo) => todo.id === this.editTodoForm.todo.id
      );
      // update the title of the todo with the new title provided in the edit form
      todo.title = this.editTodoForm.todo.title;
      // hides the edit form
      this.editTodoForm.show = false;
    },
    async deleteTodo(id) {
      await axios.delete(`/api/todos/${id}`);
      this.fetchTodos();
    },
  },
};
</script>

<template>
  <Navbar />

  <main class="container">
    <Modal 
    :show="editTodoForm.show" 
    @close="editTodoForm.show = false">
      <template v-slot:header>
        <h2>Edit Todo</h2>
      </template>

      <template v-slot:content>
        <form class="edit-todo-form">
          <div><label>Todo Title</label></div>
          <input type="text" v-model="editTodoForm.todo.title" />
        </form>
      </template>

      <template v-slot:footer>
        <div class="edit-todo-modal-footer">
          <Btn class="edit-todo-submit-btn" 
          @click="updateTodo">Submit</Btn>
          <Btn type="danger" 
          @click="editTodoForm.show = false">Close</Btn>
        </div>
      </template>
    </Modal>

    <Alert 
    :message="alert.message" 
    :show="alert.show" 
    :type="alert.type"
    @close="alert.show = false" />

    <section>
      <AddTodoForm 
      @submit="addTodo" 
      :isLoading="isPostingTodo"
      />
    </section>

    <section>
      <Spinner v-if="isLoading" class="spinner" />
      <div v-else>
        <Todo
          v-for="todo in todos" 
          :key="todo.id" 
          :title="todo.title" 
          @delete="deleteTodo(todo.id)"
          @edit="showEditTodoForm(todo)" />
      </div>
    </section>
  </main>
</template>

<style scoped>
.spinner{
  margin: auto;
  margin-top: 10rem;
}
.edit-todo-form>input {
  width: 100%;
  height: 30px;
  border: 1px solid var(--accent-color);
}

.edit-todo-modal-footer {
  display: flex;
  justify-content: end;
  padding: 10px;
}

.edit-todo-submit-btn {
  margin-right: 5px;
}
</style>