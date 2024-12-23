<template>
  <li>
    <input type="checkbox" v-bind:checked="task.completed" v-on:change="$emit('complete')" />

    <span v-if="!isEditing" v-bind:class="{ completed: task.completed, overdue: isOverdue }">
      {{ task.name }} <small>({{ task.category }})</small> - <small>Due: {{ task.dueDate }}</small>
    </span>

    <!-- Input field for editing the task -->
    <input v-if="isEditing" ref="editInput" v-model="editName" v-on:keyup.enter="saveEdit" />

    <!-- Edit and Delete Buttons -->
    <button v-on:click="toggleEdit">{{ isEditing ? 'Save' : 'Edit' }}</button>
    <button v-if="!isEditing" v-on:click="$emit('delete')">Delete</button>
  </li>
</template>

<script>
import { nextTick } from 'vue'

export default {
  props: ['task'],
  data() {
    return {
      isEditing: false,
      editName: this.task.name,
    }
  },
  methods: {
    toggleEdit() {
      if (this.isEditing) {
        this.saveEdit()
      } else {
        this.isEditing = true

        nextTick(() => {
          const editInput = this.$refs.editInput
          editInput.focus()
        })
      }
    },
    saveEdit() {
      if (this.editName.trim() && this.editName.trim() !== this.task.name) {
        this.$emit('edit', { id: this.task.id, editedName: this.editName })
      }

      this.isEditing = false
    },
  },
  computed: {
    isOverdue() {
      return new Date(this.task.dueDate) < new Date() && !this.task.completed
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
</style>
