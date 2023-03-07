<script>
import Btn from "./btn.vue";
import Spinner from "./spinner.vue";

export default {
  components: {
    Btn,
    Spinner
},
  props:{
      isLoading:{
      disabled:{
        default: false,
        type: Boolean
      }
    }
  },
  data() {
    return {
      todoTitle: "",
    };
  },
  emits: ['submit'],
};
</script>

<template>
  <form class="add-todo-form">
    <input v-model="todoTitle" type="text" placeholder="Todo Title" />
    <div>
      <Btn
      class="btn" 
      :disabled="isLoading"
      @click="$emit('submit', todoTitle)">
        <Spinner v-if="isLoading" />
        <span v-else> Add Todo</span>
      </Btn>
    </div>
  </form>
</template>

<style scoped>
.add-todo-form {
  display: flex;
  justify-content: space-between;
  margin-top: 2rem;
}

.add-todo-form input {
  width: 80%;
  border: solid 2px var(--accent-color);
}

.btn {
  height: 50px;
  color: white;
}
</style>