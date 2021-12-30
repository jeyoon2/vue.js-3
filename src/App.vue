<template>
  <div class="container">
    <h2>To-Do List</h2>
    <form @submit.prevent="onSubmit" class="d-flex">
      <div class="flex-grow-1 mr-2">
        <input
          class="form-control"
          type="text"
          v-model="todo"
          placeholder="Type new to-do"
        />
      </div>

      <div>
        <button class="btn btn-primary" type="submit">Add</button>
      </div>
    </form>
    <!-- {{ todos }} -->
    <div v-for="todo in todos" :key="todo.id" class="card mt-2">
      <div class="card-body p-2">{{ todo.subject }}</div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const todo = ref("");
    const todos = ref([
      { id: 1, subject: "휴대폰 사기" },
      { id: 2, subject: "장보기" },
    ]);
    // ref를 사용한 todos 데이터는 reactive 하기 때문에 todos 데이터가 변경되면 화면 템플릿의 데이터도 변경된다

    const onSubmit = () => {
      console.log(todo.value);
      todos.value.push({ id: Date.now(), subject: todo.value });
    };

    return {
      todo,
      todos,
      onSubmit,
    };
  },
};
</script>

<style>
.name {
  color: green;
}
</style>
