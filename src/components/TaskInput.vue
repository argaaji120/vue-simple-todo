<template>
  <div>
    <input v-model="name" @keyup.enter="submitTask" placeholder="Add a new task" />

    <select v-model="category">
      <option disabled value="">Select Category</option>
      <option>Work</option>
      <option>Personal</option>
    </select>

    <input v-model="dueDate" type="date" />

    <select v-model="priority">
      <option value="High">High</option>
      <option value="Medium">Medium</option>
      <option value="Low">Low</option>
    </select>

    <button @click="submitTask">Add Task</button>

    <br />
    <small v-if="inputError" class="error-message">Please fill out all fields</small>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: '',
      category: '',
      dueDate: '',
      priority: 'Medium',
      inputError: false,
    }
  },
  methods: {
    submitTask() {
      if (this.name.trim() && this.category && this.dueDate) {
        this.$emit('addTask', {
          name: this.name,
          completed: false,
          category: this.category,
          dueDate: this.dueDate,
          priority: this.priority,
        })

        this.name = ''
        this.category = ''
        this.dueDate = ''
        this.priority = 'Medium'
        this.inputError = false
      } else {
        this.inputError = true
      }
    },
  },
}
</script>

<style scoped>
input {
  margin-right: 7px;
}

select {
  margin-right: 7px;
}

.error-message {
  color: red;
}
</style>
