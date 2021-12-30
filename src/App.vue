<template>
  <div class="name">{{ greet }}</div>
</template>

<script>
export default {
  setup() {
    const name = "Jeyoon";

    const greeting = (name) => {
      return `Hello, ${name}`;
    };

    const greet = greeting(name);

    return {
      greet,
    };
  },
};
</script>

<style>
.name {
  color: green;
}
</style>
