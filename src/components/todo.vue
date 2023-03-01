<script>
import Btn from "./btn.vue";
import Pencil from "./icons/pencil.vue";

export default {
  components:{
    Btn,
    Pencil
},
  props: {
    title: {
      required: true,
      type: String,
    },
  },
  emits: ['delete', 'edit']
};
</script>

<template>
  <div class="todo">
    <p>{{ title }}</p>
    <div>
      <Btn 
        circle 
        type="secondary" 
        @click="$emit('edit')" 
        class="btn edit-todo-btn">
        <Pencil />
      </Btn>
      <Btn 
        circle 
        type="danger" 
        @click="$emit('delete')" 
        class="btn">
        &times;
      </Btn>
    </div>
  </div>
</template>

<style scoped>
.todo {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: var(--accent-color);
  margin-top: 30px;
  padding: 0 20px 0 20px;
  border-radius: 10px;
}

.todo > div{
  display: flex;
}

.edit-todo-btn{
  margin-right: 0.5rem;
}
.btn {
  border-radius: 50%;
  border: none;
  height: 40px;
  width: 40px;
  font-size: 30px;
  color: var(--text-color);
  background: var(--danger-color);
  cursor: pointer
}
</style>