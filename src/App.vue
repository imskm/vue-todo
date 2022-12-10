<template>
  <h1>Todo</h1>
  <form @submit.prevent="addTodoItem" action="">
    <input ref="inputBox" v-model="todoText" type="text" placeholder="Type anything.." autofocus="true">
    <button type="submit">Add</button>
  </form>
  <ul>
    <li v-for="item in todoItems">
      <label :class="{'done': item.isDone}">
        <input v-model="item.isDone" type="checkbox" name="check" id="check">
        {{ item.text }}
      </label>
    </li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      todoText: "",
      todoItems: [],
    }
  },
  methods: {
    addTodoItem() {
      if (this.todoText.replace(/ /g, "").length === 0) {
        return;
      }

      let todoItem = {
        text: this.todoText,
        isDone: false,
      };

      this.todoItems.push(todoItem);
      this.todoText = "";
      this.$refs.inputBox.focus();

      localStorage.setItem("todo-store", JSON.stringify(this.todoItems));
    }
  },

  mounted() {
    let store = localStorage.getItem("todo-store");
    if (store) {
      this.todoItems = JSON.parse(store);
    }
  },
}

</script>

<style scoped>
.done {
  color: #888888;
  text-decoration: line-through;
}
</style>
