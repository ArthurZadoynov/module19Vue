<script>
import Item from "./Item.vue";

let id = 0;

export default {
  data() {
    return {
      todos: [],
      notification: "", // Состояние для уведомления
      showNotification: false, // Состояние для управления видимостью уведомления
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
        this.saveTodos();
        this.showNotificationMessage("Задача успешно добавлена!");
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
        this.saveTodos();
        this.showNotificationMessage("Задача успешно добавлена!");
        input.value = "";
      }
    },

    deleteTodo(id) {
      this.todos.splice(
        this.todos.findIndex((todo) => id === todo.id),
        1
      );
      this.saveTodos();
      this.showNotificationMessage("Задача успешно удалена!");
    },

    showNotificationMessage(message) {
      this.notification = message;
      this.showNotification = true;
      setTimeout(() => {
        this.showNotification = false;
        this.notification = "";
      }, 2000);
    },

    saveTodos() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },

    loadTodos() {
      const storedTodos = localStorage.getItem("todos");
      if (storedTodos) {
        this.todos = JSON.parse(storedTodos);
        id = this.todos.length
          ? Math.max(...this.todos.map((todo) => todo.id)) + 1
          : 0;
      }
    },
  },
  mounted() {
    this.loadTodos();
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
      <button class="addTaskBtn" @click="addTodoBtn">Добавить задачу</button>
    </div>
    <Item
      v-for="todo in renderTodos"
      :key="todo.id"
      :todo="todo"
      v-on:del="deleteTodo"
    />

    <div v-if="showNotification" class="notification-container">
      <div class="notification">{{ notification }}</div>
    </div>
  </section>
</template>

<style scoped>
.real-app {
  display: flex;
  flex-direction: column;
  position: relative;
  padding: 20px 40px;
  max-width: 1920px;
  margin: 0 auto;
  box-shadow: 0 0 5px #666;
  width: calc(100% - 80px);
}

.addTask {
  display: flex;
  justify-content: space-between;
  gap: 20px;
  align-items: center;

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
  max-width: calc(100% - 40px);
  width: 100%;
}

.addTaskBtn {
  border: none;
  outline: none;
  color: #fff;
  background: #e0e0e0;
  cursor: pointer;
  position: relative;
  z-index: 0;
  border-radius: 10px;
  font-size: 30px;
  padding: 15px;
}

.addTaskBtn:before {
  content: "";
  background: linear-gradient(
    45deg,
    #ff0000,
    #ff7300,
    #fffb00,
    #48ff00,
    #00ffd5,
    #002bff,
    #7a00ff,
    #ff00c8,
    #ff0000
  );
  position: absolute;
  top: -2px;
  left: -2px;
  background-size: 400%;
  z-index: -1;
  filter: blur(5px);
  width: calc(100% + 4px);
  height: calc(100% + 4px);
  animation: glowing 20s linear infinite;
  opacity: 0;
  transition: opacity 0.3s ease-in-out;
  border-radius: 10px;
}

.addTaskBtn:active {
  color: #000;
}

.addTaskBtn:active:after {
  background: transparent;
}

.addTaskBtn:hover:before {
  opacity: 1;
}

.addTaskBtn:after {
  z-index: -1;
  content: "";
  position: absolute;
  width: 100%;
  height: 100%;
  background: #3f3939;
  left: 0;
  top: 0;
  border-radius: 10px;
}

.notification-container {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(0, 0, 0, 0.5);
}

.notification {
  padding: 20px;
  background-color: #4caf50;
  color: white;
  border-radius: 5px;
}

@keyframes glowing {
  0% {
    background-position: 0 0;
  }
  50% {
    background-position: 400% 0;
  }
  100% {
    background-position: 0 0;
  }
}

@media (max-width: 780px) {
  .real-app {
    padding: 20px 8px;
  }

  .addTask {
    flex-direction: column;
    gap: 10px;

    input {
      border: 1px solid #fff;
    }

    button {
      width: 100%;
      padding: 15px 0;
      margin-bottom: 40px;
    }
  }
}

@media (max-width: 560px) {
  .real-app {
    width: calc(100% - 40px);
  }
}
</style>
