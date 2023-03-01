<script>
import Btn from "./btn.vue";

export default {
  components: {
    Btn,
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
      <Btn class="btn" @click="$emit('submit', todoTitle)">
        Add Todo
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