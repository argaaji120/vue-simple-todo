<template>
  <div class="row g-3">
    <div class="col-5">
      <input class="form-control" type="text" placeholder="Add a new Task" v-model="name" />
    </div>

    <div class="col">
      <select class="form-select" v-model="category">
        <option disabled value="">Select Category</option>
        <option>Work</option>
        <option>Personal</option>
      </select>
    </div>

    <div class="col">
      <input class="form-control" type="date" v-model="dueDate" />
    </div>

    <div class="col">
      <select class="form-select" v-model="priority">
        <option value="High">High</option>
        <option value="Medium">Medium</option>
        <option value="Low">Low</option>
      </select>
    </div>

    <div class="col">
      <button class="btn btn-success" v-on:click="submitTask">Add Task</button>
    </div>
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
