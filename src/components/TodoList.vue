<template>
  <div class="container">
    <div class="section">
      <div class="is-centered">
        <div>
          <h1 class="title">Todo List</h1>
        </div>

        <b-collapse :open="false" aria-id="contentIdForA11y1">
          <template #trigger>
            <b-button label="Add New" type="is-text" aria-controls="contentIdForA11y1" />
          </template>
          <div class="notification">
            <div class="content">
              <div class="columns">
                <b-input class="column is-three-quarters has-text-left ml-4" v-model="newTodo" placeholder="enter new task" />
                <div class="column">
                  <b-button type="is-success" @click="handleAdd(todo)" :disabled="newTodo == ''">Add</b-button>
                </div>
              </div>

            </div>
          </div>
        </b-collapse>
      </div>
      <b-loading :is-full-page="false" v-model="isLoading"></b-loading>
      <ul>
        <li v-for="todo in todoes" :key="todo.Id" @click="handleClick(todo)">
          <div class=" columns is-vcentered has-background-light m-1">
            <b-input class="column is-three-quarters has-text-left ml-4" v-model="todo.Name" :disabled="isClicked != todo.Id" />
            <b-checkbox size="is-large" v-model="todo.IsComplete" :disabled="isClicked != todo.Id" class="column" />
            <div class="column">
              <b-button type="is-success" outlined icon-right="content-save-outline" @click.stop="handleSave(todo)" :disabled="isClicked != todo.Id" />
            </div>
            <div class="column">
              <b-button type="is-danger" outlined icon-right="close" @click.stop="handleDelete(todo)" />
            </div>
          </div>
        </li>
      </ul>
    </div>

  </div>
</template>

<script>
import Vue from "vue";
import Buefy from "buefy";
import "buefy/dist/buefy.css";

Vue.use(Buefy);

export default {
  name: "TodoList",
  props: { todoes: [], isLoading: Boolean },

  data: function () {
    return {
      isClicked: 0,
      todoName: String,
      todoDone: Boolean,
      newTodo: "",
      todo: {},
    };
  },

  methods: {
    handleClick(todo) {
      this.isClicked = todo.Id;
    },
    handleSave(todo) {
      this.$emit("onSave", todo);
      this.isClicked = 0;
    },

    handleDelete(todo) {
      this.$emit("onDelete", todo);
      this.isClicked = 0;
    },

    handleAdd() {
      this.$emit("onAdd", { Name: this.newTodo, IsComplete: false });
      this.isClicked = 0;
      this.newTodo = "";
    },
  },
};
</script>