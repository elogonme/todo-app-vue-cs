<template>
  <div class="container is-max-desktop">
    <div class="columns is-centered is-vcentered">
      <img alt="Vue logo" src="../assets/logo.png" class="image is-48x48">
      <h1 class="title">Todo App with ODATA</h1>
    </div>
    <TodoList :todoes="todoes" :isLoading="isLoading" @onSave="saveTodo" @onAdd="addTodo" @onDelete="deleteTodo" />
  </div>
</template>

<script>
// @ is an alias to /src
import Vue from "vue";
import Buefy from "buefy";
import "buefy/dist/buefy.css";

Vue.use(Buefy);
import TodoList from "@/components/TodoList.vue";

export default {
  name: "Home",
  components: {
    TodoList,
  },

  data: function () {
    return {
      todoes: [],
      isLoading: true,
    };
  },

  created() {
    this.getTodos();
  },

  methods: {
    addTodo(todo) {
      this.isLoading = true;
      fetch(`http://localhost:49851/odata/Todo`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(todo),
      }).then(() => this.getTodos());
    },
    getTodos() {
      (this.isLoading = true),
        fetch("http://localhost:49851/odata/Todo")
          .then((res) => res.json())
          .then((data) => {
            this.todoes = data.value;
            this.isLoading = false;
          });
    },
    saveTodo(todo) {
      this.isLoading = true;
      fetch(`http://localhost:49851/odata/Todo(${todo.Id})`, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(todo),
      }).then(() => this.getTodos());
    },

    deleteTodo(todo) {
      this.isLoading = true;
      fetch(`http://localhost:49851/odata/Todo(${todo.Id})`, {
        method: "DELETE",
      }).then(() => this.getTodos());
    },
  },
};
</script>
