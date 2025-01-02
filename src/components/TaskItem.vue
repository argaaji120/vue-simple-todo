<template>
  <li>
    <input type="checkbox" v-bind:checked="task.completed" v-on:change="$emit('complete')" />

    <span
      v-if="!isEditing"
      v-bind:class="{
        completed: task.completed,
        overdue: isOverdue,
        'due-soon': isDueSoon && !task.completed,
      }"
    >
      {{ task.name }}
      <small>({{ task.category }})</small>
      <small v-if="task.dueDate"> - Due: {{ task.dueDate }}</small>
      <small v-if="task.priority" class="priority">{{ task.priority }}</small>
    </span>

    <!-- Edit and Delete Buttons -->
    <button v-if="!task.completed" v-on:click="editTask">Edit</button>
    <button v-on:click="$emit('delete')">Delete</button>

    <!-- Notification for tasks due soon -->
    <small v-if="isDueSoon && !task.completed" class="due-soon-notification">
      This task is due soon!
    </small>
  </li>
</template>

<script>
export default {
  props: ['task'],
  data() {
    return {
      isEditing: false,
      editName: this.task.name,
    }
  },
  methods: {
    editTask() {
      this.$emit('edit', this.task)
    },
  },
  computed: {
    isOverdue() {
      return new Date(this.task.dueDate) < new Date() && !this.task.completed
    },
    isDueSoon() {
      const dueDate = new Date(this.task.dueDate)
      const now = new Date()
      const timeDifference = dueDate - now
      const oneDayInMillis = 24 * 60 * 60 * 1000

      return timeDifference <= oneDayInMillis && timeDifference > 0
    },
  },
}
</script>

<style scoped>
ul li {
  line-height: 30px !important;
}

input[type='checkbox'] {
  cursor: pointer;
}

span {
  font-size: 16px;
}

button {
  margin-left: 7px;
  cursor: pointer;
}

.completed {
  text-decoration: line-through;
  font-style: italic;
  color: gray;
}

.overdue {
  color: red;
  font-weight: bold;
}

.due-soon {
  color: orange;
}

.due-soon-notification {
  color: red;
  font-size: 12px;
  margin-left: 5px;
}

.priority {
  font-weight: bold;
  color: blue;
  margin-left: 10px;
}
</style>
