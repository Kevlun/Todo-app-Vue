<template>
  <form @submit="onSubmit" class="todo-form" v-if="!showEditTodo">
    <label for="">To-do</label>
    <input
      name="text"
      type="text"
      placeholder="Add to-do"
      autoComplete="off"
      v-model="text"
      @change="$emit('edit-todo')"
    />
    <label>Priority</label>
    <select v-model="priority" name="priority" id="priority">
      <option value="" selected disabled hidden>Select priority</option>
      <option value="High priority" class="high">High priority</option>
      <option value="Medium priority" class="med">Medium priority</option>
      <option value="Low priority" class="low">Low priority</option>
    </select>

    <label>Day & Time</label>
    <div class="date">
      <input
        v-model="date"
        name="date"
        type="date"
        placeholder="Add Day & Time"
      />
      <input
        v-model="time"
        name="time"
        type="time"
        placeholder="Add Day & Time"
      />
    </div>
    <input type="submit" value="Add todo" class="submit" />
  </form>
  <form @submit="onSubmit" class="todo-form edit" v-else>
    <label for="">Update Todo</label>
    <input
      name="text"
      type="text"
      placeholder="Add to-do"
      autoComplete="off"
      v-model="this.edit.text"
    />
    <label>Set new Priority</label>
    <select v-model="priority" name="priority" id="priority">
      <option value="" selected disabled hidden>Select priority</option>
      <option value="High priority" class="high">High priority</option>
      <option value="Medium priority" class="med">Medium priority</option>
      <option value="Low priority" class="low">Low priority</option>
    </select>
    <label>Set new Day & Time</label>
    <div class="date">
      <input
        v-model="this.edit.date"
        name="date"
        type="date"
        placeholder="Add Day & Time"
      />
      <input
        v-model="this.edit.time"
        name="time"
        type="time"
        placeholder="Add Day & Time"
      />
    </div>
    <input type="submit" value="Update todo" class="submit" />
  </form>
</template>

<script>
import Button from "./Button.vue";
export default {
  components: { Button },
  name: "AddTodo",
  props: {
    showEditTodo: Boolean,
    edit: Object,
    todo: String,
    toggleEdit: Function,
  },

  data() {
    return {
      id: "",
      text: "",
      date: "",
      time: "",
      priority: "",
      done: false,
      editTodo: false,
    };
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();

      const newTodo = {
        id: Math.floor(Math.random() * 100000),
        text: this.showEditTodo ? this.edit.text : this.text,
        date: this.showEditTodo ? this.edit.date : this.date,
        time: this.showEditTodo ? this.edit.time : this.time,
        priority: this.priority,
        done: this.done,
        editTodo: false,
      };
      this.$emit("add-todo", newTodo);
      this.id = "";
      this.text = "";
      this.date = "";
      this.time = "";
      this.priority = "";
      if (this.showEditTodo) {
        this.$emit("toggleEdit");
      }
    },
  },
};
</script>

<style>
.date {
  display: flex;
  flex-direction: row;
}

.date input {
  width: 100%;
}

.todo-form {
  display: flex;
  flex-direction: column;
}

.high {
  background-color: rgba(255, 0, 0, 0.7);
}

.med {
  background-color: rgba(0, 0, 255, 0.5);
}

.low {
  background-color: rgba(0, 128, 0, 0.3);
}

label {
  margin: 5px;
  font-size: 1.2em;
}

input,
select {
  /* width: 100%; */
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}

.submit {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 18px;
  font-family: inherit;
}

.edit input,
.edit select {
  color: rgba(79, 89, 182, 0.986);
}
</style>
