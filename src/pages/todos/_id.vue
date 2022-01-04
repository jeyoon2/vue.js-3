<template>
  <h1>To-Do Page</h1>
  <div v-if="loading">Loading...</div>
  <form v-else @submit.prevent="onSave">
    <div class="row">
      <div class="col-6">
        <div class="form-group">
          <label>Subject</label>
          <input v-model="todo.subject" type="text" class="form-control" />
        </div>
      </div>
      <div class="col-6">
        <div class="form-group">
          <label>Status</label>
          <div>
            <button
              class="btn"
              type="button"
              :class="todo.completed ? 'btn-success' : 'btn-danger'"
              @click="toggleTodoStatus"
            >
              {{ todo.completed ? "Completed" : "Incomplete" }}
            </button>
          </div>
        </div>
      </div>
    </div>

    <button type="submit" class="btn btn-primary" :disabled="!todoUpdated">
      Save
    </button>
    <button class="btn btn-outline-dark ml-2" @click="moveToTodoListPage">
      Cancel
    </button>
    <Toast v-if="showToast" :message="toastMessage" :type="toastAlertType" />
  </form>
  <div id="Jeyoon"></div>
</template>

<script>
import { useRoute, useRouter } from "vue-router";
import axios from "axios";
import {
  ref,
  computed,
  onBeforeMount,
  onMounted,
  onBeforeUpdate,
  onUpdated,
  onBeforeUnmount,
  onUnmounted,
} from "vue";
import _ from "lodash";
import Toast from "@/components/Toast.vue";

export default {
  components: {
    Toast,
  },
  setup() {
    onBeforeMount(() => {
      // 아직 돔에 올라가지 않은 상태. 마운트 되기 전 실행
      // setup 내의 모든 코드를 훑고 난 후 실행됨
      console.log(document.querySelector("#Jeyoon"));
    });

    onMounted(() => {
      // 마운트 되고 난 후 실행
      console.log(document.querySelector("#Jeyoon"));
    });

    onBeforeUpdate(() => {
      // 업데이트가 되기 전 실행되는 로직
      console.log("before update");
    });

    onUpdated(() => {
      // state가 업데이트될 때마다 실행되는 로직
      console.log("updated");
    });

    onBeforeUnmount(() => {
      // 언마운트 되기 전 실행
      console.log("before unmount");
    });

    onUnmounted(() => {
      // 언마운트 (다른 페이지로 이동) 시 실행.
      // 주로 페이지 이동 전 메모리 정리를 위해 사용
      console.log("unmouted");
    });

    console.log("hello");

    const route = useRoute();
    const router = useRouter();
    const todo = ref(null);
    const originalTodo = ref(null);
    const loading = ref(true);
    const showToast = ref(true);
    const toastMessage = ref("");
    const toastAlertType = ref("");
    const todoId = route.params.id;

    const getTodo = async () => {
      try {
        const res = await axios.get(`http://localhost:3000/todos/${todoId}`);

        todo.value = { ...res.data };
        originalTodo.value = { ...res.data };

        loading.value = false;
      } catch (error) {
        console.log(error);
        triggerToast("Something went wrong");
      }
    };

    const todoUpdated = computed(() => {
      return !_.isEqual(todo.value, originalTodo.value);
    });

    const toggleTodoStatus = () => {
      todo.value.completed = !todo.value.completed;
    };

    const moveToTodoListPage = () => {
      router.push({
        name: "Todos",
      });
    };

    getTodo();

    const triggerToast = (message, type = "success") => {
      toastMessage.value = message;
      toastAlertType.value = type;
      showToast.value = true;
      setTimeout(() => {
        toastMessage.value = "";
        toastAlertType.value = "";
        showToast.value = false;
      }, 3000);
    };

    const onSave = async () => {
      try {
        const res = await axios.put(`http://localhost:3000/todos/${todoId}`, {
          subject: todo.value.subject,
          completed: todo.value.completed,
        });

        originalTodo.value = { ...res.data };
        triggerToast("Successtully saved!");
      } catch (err) {
        console.log(err);
        triggerToast("Something went wrong", "danger");
      }
    };

    return {
      todo,
      originalTodo,
      loading,
      todoUpdated,
      toggleTodoStatus,
      moveToTodoListPage,
      onSave,
      showToast,
      toastMessage,
      toastAlertType,
    };
  },
};
</script>

<style></style>
