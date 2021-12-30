<template>
  <div class="name">{{ name }}</div>
  <button class="btn btn-primary" v-on:click="updateName">Click</button>
</template>

<script>
// import {ref} from "vue" // 화면 동시에 업데이트. 문자열 일때
import { reactive } from "vue"; // 배열이나 객체일 때 주로 사용

export default {
  setup() {
    const name = reactive({ id: 1 });

    // const greeting = (name) => {
    //   return `Hello, ${name}`;
    // };

    // const greet = greeting(name);

    const updateName = () => {
      // name.value = Jeyoon
      name.id = 2;
      console.log(name);
    };

    return {
      name,
      updateName,
    };
  },
};
</script>

<style>
.name {
  color: green;
}
</style>
