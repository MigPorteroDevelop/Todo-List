<script>
export default {
  props: {
    type: {
      required: false,
      default: "success",
      validator(value) {
        const options = ["danger", "warning", "info", "success", "secondary"];
        return options.includes(value);
      },
      applyCircleClass(){
        return this.circle;
      }
    },
    circle: {
      default: false,
      type: Boolean,
    },
  },
  computed: {
    backgroundColor() {
      const options = {
        danger: "var(--danger-color)",
        info: "var(--info-color)",
        warning: "var(--warning-color)",
        success: "var(--accent-color)",
        secondary: "var(--secondary-color)",
      };
      return options[this.type];
    },
    applyCircleClass() {
      return this.circle;
    },
  },
  emits: ["click"],
};
</script>

<template>
  <button :style="{ backgroundColor }" @click.prevent="$emit('click')" :class="{ circle: applyCircleClass }">
    <!--This is where you put the things you send from outside-->
    <slot />
  </button>
</template>

<style scoped>
button{
  color: var(--text-color);
  border: none;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
}
.circle {
  border-radius: 50%;
}
</style>