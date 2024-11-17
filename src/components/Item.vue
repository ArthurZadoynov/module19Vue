<script>
import DeleteBtnSvg from "../assets/deleteBtn.svg";

export default {
  components: {
    DeleteBtnSvg,
  },

  props: {
    todo: {
      type: Object,
    },
  },

  methods: {
    deleteTodo() {
      this.$emit("del", this.todo.id);
    },
  },
};
</script>
<template>
  <div class="todo-item">
    <p v-if="todo.checked">
      <del>{{ todo.content }}</del>
    </p>
    <p v-else>{{ todo.content }}</p>
    <div>
      <button class="delete-btn" @click="deleteTodo"><DeleteBtnSvg /></button>
      <label class="custom-checkbox">
        <input type="checkbox" v-model="todo.checked" />
        <span class="checkmark"></span>
      </label>
    </div>
  </div>
</template>

<style scoped>
.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 20px 0;
  position: relative;
  font-size: 24px;
  border-bottom: 1px solid rgb(197, 196, 196);
  gap: 20px;
  word-wrap: break-word;

  div {
    display: flex;
    gap: 20px;
  }

  p {
    text-align: left;
    max-width: 600px;
    width: 100%;
  }

  > p del {
    color: rgb(0, 143, 21);
  }

  label {
    display: flex;
    align-items: center;

    input {
      cursor: pointer;
      width: 30px;
      height: 30px;
    }
  }

  .custom-checkbox {
    position: relative;
    display: flex;
    align-items: center;
  }

  .custom-checkbox input {
    display: none;
  }

  .checkmark {
    width: 30px;
    height: 30px;
    background-color: #eee;
    border-radius: 5px;
    display: inline-block;
    position: relative;
  }

  .custom-checkbox input:checked + .checkmark {
    background-color: rgb(0, 143, 21);
  }

  .custom-checkbox input:checked + .checkmark::after {
    content: "";
    position: absolute;
    left: 12px;
    top: 5px;
    width: 5px;
    height: 15px;
    border: solid white;
    border-width: 0 3px 3px 0;
    transform: rotate(45deg);
  }

  .delete-btn {
    position: relative;
    width: 70px;
    height: 70px;
    border-radius: 45px;
    border: 2px solid rgb(231, 50, 50);
    background-color: #fff;
    cursor: pointer;
    box-shadow: 0 0 10px #333;
    overflow: hidden;
    transition: 0.3s;
    transform-origin: center;

    svg {
      color: rgb(231, 50, 50);
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%) scale(1.1);
      width: 40px;
      height: 40px;
    }

    &:hover {
      background-color: rgb(245, 207, 207);
      transform: scale(1.1);
      box-shadow: 0 0 4px #d8d8d8;
      transition: 0.3s;
    }
  }
}
</style>
