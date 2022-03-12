<template>
  <div :class="switchTheme ? 'container dark-theme' : 'container'">
    <video
      v-if="switchTheme"
      src="./assets/video-1.mp4"
      autoPlay
      loop
      muted
    ></video>
    <div class="todo-card">
      <div class="status">
        <div>
          <label>Filter</label>
          <select v-model="prio" value="prio" name="prio" id="">
            <option value="" selected>By Priority</option>
            <option value="High priority">High priority</option>
            <option value="Medium priority">Medium priority</option>
            <option value="Low priority">Low priority</option>
          </select>
          <button @click="hideCompleted = !hideCompleted">
            {{ hideCompleted ? "Show all" : "Hide completed" }}
          </button>
          <Button
            @click="toggleTheme"
            :text="switchTheme ? 'Light-mode' : 'Dark-mode'"
          />
          <Button
            @click="toggleAdd"
            :text="showAddTodo ? 'Close add-todo' : 'Add-todo'"
            :color="showAddTodo ? 'red' : 'green'"
          />
        </div>

        <button class="delete-btn" @click="clearAll">Delete all</button>
      </div>
      <AddTodo
        @add-todo="addTodo"
        v-show="showAddTodo"
        :showEditTodo="showEditTodo"
        :edit="edit"
        :todo="todo"
        @toggleEdit="toggleEdit"
      />
      <Todos
        :filteredTodos="hideCompleted ? filteredTodos : filteredTodos2"
        v-model="todos"
        @delete-todo="deleteTodo"
        @complete-todo="completeTodo"
        @edit-todos="editTodos"
        :showEditTodo="showEditTodo"
        @toggleEdit="toggleEdit"
        :switchTheme="switchTheme"
      />
    </div>
  </div>
</template>

<script>
import AddTodo from "./components/AddTodo.vue";
import Todos from "./components/Todos.vue";
import Button from "./components/Button.vue";
import "./global.css";
export default {
  name: "App",
  components: {
    AddTodo,
    Todos,
    Button,
  },
  data() {
    return {
      todos: [],
      showAddTodo: false,
      showEditTodo: false,
      hideCompleted: false,
      switchTheme: false,
      prio: "",
      index: "",
      edit: {},
      todo: "",
    };
  },
  mounted() {
    if (localStorage.todos) {
      this.todos = JSON.parse(localStorage.todos);
    }
  },
  watch: {
    todos: {
      handler(newTodos) {
        localStorage.todos = JSON.stringify(newTodos);
        console.log(newTodos);
      },
      deep: true,
    },
  },

  computed: {
    filteredTodos() {
      return this.hideCompleted
        ? this.todos.filter((todo) => !todo.done)
        : this.todos;
    },
    filteredTodos2() {
      return this.prio
        ? this.todos.filter((todo) => todo.priority === this.prio)
        : this.todos;
    },
  },
  methods: {
    editTodos(id, text, date, time, priority) {
      this.showAddTodo = !this.showAddTodo;
      this.showAddTodo = true;
      this.showEditTodo = true;
      this.edit = {
        id: id,
        text: text,
        date: date,
        time: time,
        prio: priority,
      };
      this.todo = text;
      this.deleteTodo(id);
    },
    clearAll() {
      this.todos = [];
    },

    toggleEdit() {
      this.showEditTodo = !this.showEditTodo;
    },
    toggleTheme() {
      this.switchTheme = !this.switchTheme;
    },
    toggleAdd() {
      this.showAddTodo = !this.showAddTodo;
    },
    completeTodo(id) {
      this.todos = this.todos.map((todo) =>
        todo.id === id ? { ...todo, done: !todo.done } : todo
      );
    },

    addTodo(todo) {
      this.todos = [...this.todos, todo];
    },
    deleteTodo(id) {
      this.todos = this.todos.filter((todo) => todo.id !== id);
    },
  },
};
</script>

<style scoped>
body {
  min-height: 100vh;
}
.todo-card {
  padding: 2em;
}
.container {
  display: grid;
  min-height: 100vh;
}

.dark-theme label {
  color: white;
}

.delete-btn {
  background: red;
}
h1 {
  font-family: monospace;
  font-weight: lighter;
  text-transform: uppercase;
}

button {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 12px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: monospace;
}

video {
  object-fit: cover;
  width: 100%;
  height: 100%;
  position: fixed;
  z-index: -1;
}

.status {
  display: flex;
  flex-direction: row;
  align-items: center;
  align-self: center;
  justify-content: space-between;
}

select {
  border-radius: 6px;
  padding: 0em 0.8em;
}

label {
  font-family: monospace;
  font-weight: bolder;
  font-size: 1.2em;
}

option {
  font-family: monospace;
}
</style>
