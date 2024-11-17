<script>
import Item from "./Item.vue";

let id = 0;

export default {
  data() {
    return {
      todos: [],
    };
  },
  components: {
    Item,
  },
  computed: {
    renderTodos() {
      return this.todos;
    },
  },
  methods: {
    addTodo(e) {
      if (e.target.value.trim()) {
        this.todos.unshift({
          id: id++,
          content: e.target.value,
          checked: false,
        });
        e.target.value = "";
      }
    },
    addTodoBtn() {
      const input = this.$refs.input;
      if (input.value.trim()) {
        this.todos.unshift({
          id: id++,
          content: input.value,
          checked: false,
        });
        input.value = "";
      }
    },

    deleteTodo(id) {
      this.todos.splice(
        this.todos.findIndex((todo) => id === todo.id),
        1
      );
    },
  },
};
</script>

<template>
  <section class="real-app">
    <div class="addTask">
      <input
        ref="input"
        v-on:keyup.enter="addTodo"
        type="text"
        class="add-input"
      />
      <button @click="addTodoBtn">Добавить задачу</button>
    </div>
    <Item
      v-for="todo in renderTodos"
      :key="todo.id"
      :todo="todo"
      v-on:del="deleteTodo"
    />
  </section>
</template>

<style scoped>
.real-app {
  display: flex;
  flex-direction: column;
  position: relative;
  padding: 20px;
  width: 100%;
  max-width: 1000px;
  margin: 0 auto;
  box-shadow: 0 0 5px #666;
}

.addTask {
  display: flex;
  justify-content: space-between;
  gap: 20px;

  button {
    width: 65%;
  }
}

.add-input {
  background-color: inherit;
  border: none;
  font-size: 20px;
  border-bottom: 1px solid #fff;
  color: #fff;
  padding: 20px;
  max-width: 600px;
  width: 100%;
}
</style>
