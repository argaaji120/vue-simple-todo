<template>
  <li
    class="list-group-item d-flex justify-content-between"
    v-bind:class="{ 'bg-completed': task.completed, 'bg-overdue': isOverdue }"
  >
    <!-- Checkbox -->
    <div class="d-flex align-items-center">
      <input
        type="checkbox"
        class="form-check-input me-3"
        v-bind:checked="task.completed"
        v-on:change="$emit('complete')"
      />
      <div>
        <!-- Todo Text -->
        <p class="mb-0" :class="{ completed: task.completed, overdue: isOverdue }">
          {{ task.name }}
        </p>

        <!-- Tags -->
        <span class="badge font-10 me-1" :class="taskCategory">
          {{ task.category }}
        </span>
        <span class="badge font-10 me-1" :class="taskPriority">
          {{ task.priority }}
        </span>

        <!-- Due Date -->
        <small class="text-muted" v-if="task.dueDate">
          - <i class="fa-solid fa-calendar ms-1 me-1"></i> <i>{{ task.dueDate }}</i>
        </small>

        <small v-if="isDueSoon && !task.completed" class="due-soon-notification">
          This task is due soon!
        </small>
      </div>
    </div>

    <!-- Action Buttons -->
    <div>
      <button
        class="btn btn-link btn-sm text-black-50"
        v-if="!task.completed"
        v-on:click="editTask"
      >
        <i class="fa-solid fa-pen"></i>
      </button>

      <button class="btn btn-link btn-sm text-black-50" v-on:click="$emit('delete')">
        <i class="fa-solid fa-trash-can"></i>
      </button>
    </div>
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
    taskPriority() {
      return {
        'bg-danger': this.task.priority == 'High',
        'bg-secondary': this.task.priority == 'Low',
        'bg-warning text-black-50': this.task.priority == 'Medium',
      }
    },
    taskCategory() {
      return {
        'bg-dark': this.task.category == 'Work',
        'bg-teal': this.task.category == 'Personal',
      }
    },
  },
}
</script>

<style scoped>
input[type='checkbox'] {
  cursor: pointer;
}

button {
  margin-left: 7px;
  cursor: pointer;
}

.form-check-input {
  width: 20px;
  height: 20px;
}

.form-check-input:checked {
  background-color: #343a40;
  border-color: #343a40;
}

.form-check-input:focus {
  box-shadow: 0 0 0 0.25rem rgba(52, 58, 64, 0.25);
}

.font-10 {
  font-size: 10px;
}

.bg-teal {
  background-color: #20c997;
}

.bg-completed {
  background-color: #d1e7dd;
  border-color: #badbcc;
}

.bg-overdue {
  color: #842029;
  background-color: #f8d7da;
  border-color: #f5c2c7;
}

.completed {
  text-decoration: line-through;
  font-style: italic;
  color: #6c757d;
}

.overdue {
  /* color: #dc3545; */
  font-weight: bold;
}

.due-soon-notification {
  color: #dc3545;
  font-size: 13px;
  margin-left: 5px;
}
</style>
