<template>
  <div
    class="todo-container"
    @click="$emit('complete-todo')"
    :class="[todo.done ? 'done todo' : 'todo']"
  >
    <div class="modify">
      <h3>
        {{ todo.text }}
      </h3>
      <div class="modify-todo">
        <i @click="$emit('edit-todos')" class="fa-solid fa-pen-to-square"></i>
        <i class="fa-solid fa-check"></i>
        <i @click="$emit('delete-todo')" class="fas fa-times"></i>
      </div>
    </div>
    <div class="prio-edit-date">
      <div class="prio-date">
        <p
          :class="[
            (todo.priority === 'High priority' && 'high') ||
              (todo.priority === 'Medium priority' && 'med') ||
              (todo.priority === 'Low priority' && 'low'),
          ]"
        >
          {{ todo.priority }}
        </p>
      </div>

      <p>{{ todo.date }} {{ todo.time }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Todo",
  props: {
    todo: Object,
    showEditTodo: Boolean,
  },
};
</script>

<style>
.todo-container {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.modify {
  display: flex;
  justify-content: space-between;
}

.prio-edit {
  display: flex;
  flex-direction: row;
  gap: 1em;
  font-size: 140%;
  align-items: center;
  align-content: center;
}
.prio-edit-date {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.fas {
  color: red;
  font-size: 150%;
}

.fa-check {
  color: green;
  font-size: 140%;
}

.modify-todo {
  display: flex;
  align-self: flex-end;
  gap: 1em;
  font-size: 150%;
  align-items: center;
  align-content: center;
}

.done {
  background: #28cf123a;
}

.todo h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 1.5rem;
  font-family: monospace;
  font-weight: lighter;
  /* text-transform: uppercase; */
  margin: 0.5em 0em;
}

p {
  display: inline-flexbox;
  text-align: center;
  width: 7.5em;
  padding: 0.5em;
  border-radius: 6px;
  font-size: 1.4rem;
  margin: 0.5em 0em;
}
</style>
