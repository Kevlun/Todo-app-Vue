<template>
  <div>
    <div
      :class="
        switchTheme ? 'todo-count-container dark-theme' : 'todo-count-container'
      "
    >
      <div class="todo-count">
        <label>Status</label>
        <p>
          Done:
          {{ filteredTodos.filter((todo) => todo.done === true).length }}
        </p>
        <p>
          Left:
          {{ filteredTodos.filter((todo) => todo.done === false).length }}
        </p>
      </div>
      <div class="todo-count">
        <label>Prio</label>
        <p>
          High:
          {{
            filteredTodos.filter((todo) => todo.priority === "High priority")
              .length
          }}
        </p>
        <p>
          Medium:
          {{
            filteredTodos.filter((todo) => todo.priority === "Medium priority")
              .length
          }}
        </p>
        <p>
          Low:
          {{
            filteredTodos.filter((todo) => todo.priority === "Low priority")
              .length
          }}
        </p>
        <p class="none-prio">
          Not set:
          {{ filteredTodos.filter((todo) => todo.priority === "").length }}
        </p>
      </div>
    </div>
    <div :key="todo.id" v-for="todo in filteredTodos">
      <Todo
        @delete-todo="$emit('delete-todo', todo.id)"
        @complete-todo="$emit('complete-todo', todo.id)"
        @edit-todos="
          $emit(
            'edit-todos',
            todo.id,
            todo.text,
            todo.date,
            todo.time,
            todo.priority
          )
        "
        :todo="todo"
        :showEditTodo="showEditTodo"
      />
    </div>
  </div>
</template>

<script>
import Todo from "./Todo.vue";
export default {
  name: "Todos",
  props: {
    todos: Array,
    filteredTodos: Array,
    showEditTodo: Boolean,
    switchTheme: Boolean,
  },
  components: {
    Todo,
  },
  emits: [
    "delete-todo",
    "complete-todo",
    "edit-todo",
    "edit-todos",
    "toggleEdit",
  ],
};
</script>

<style scoped>
.todo-count-container {
  display: flex;
  flex-direction: row;
  gap: 1.2em;
}

.dark-theme {
  color: white;
}

.todo-count {
  display: flex;
  flex-direction: row;
  gap: 0.6em;
  align-items: center;
}

.none-prio {
  color: red;
}

label {
  align-self: center;
  font-weight: bold;
}

.todo-count p {
  width: auto;
  text-align: left;
  margin: 0.5em 0em;
  margin-left: 5px;
  padding: 0;
}
</style>
