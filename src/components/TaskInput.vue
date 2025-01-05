<template>
  <div>
    <button class="btn btn-success" @click="showModal">Add Todo</button>

    <MyModal ref="modal" modal-id="taskModal" title="Add New Task" v-on:close="handleClose">
      <template #body>
        <div v-if="inputError" class="text-danger mb-3">* Please fill out all fields.</div>

        <div class="mb-3">
          <label for="inputTask" class="form-label">Task</label>
          <input
            class="form-control"
            type="text"
            id="inputTask"
            placeholder="Task"
            v-model="name"
          />
        </div>

        <div class="mb-3">
          <label for="inputCategory">Category</label>
          <select class="form-select" id="inputCategory" v-model="category">
            <option disabled value="">Select Category</option>
            <option>Work</option>
            <option>Personal</option>
          </select>
        </div>

        <div class="mb-3">
          <label for="inputDueDate">Due Date</label>
          <input class="form-control" type="date" id="inputDueDate" v-model="dueDate" />
        </div>

        <div class="mb-3">
          <label for="inputPriority">Priority</label>
          <select class="form-select" id="inputPriority" v-model="priority">
            <option value="High">High</option>
            <option value="Medium">Medium</option>
            <option value="Low">Low</option>
          </select>
        </div>
      </template>

      <template #footer>
        <button class="btn btn-success" v-on:click="submitTask">Submit</button>
      </template>
    </MyModal>
  </div>
</template>

<script>
import MyModal from './MyModal.vue'

export default {
  components: {
    MyModal,
  },
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
    showModal() {
      this.$refs.modal.openModal()
    },
    handleClose() {
      this.resetForm()
      this.$refs.modal.closeModal()
    },
    submitTask() {
      if (this.name.trim() && this.category && this.dueDate) {
        this.$emit('addTask', {
          name: this.name,
          completed: false,
          category: this.category,
          dueDate: this.dueDate,
          priority: this.priority,
        })

        this.resetForm()
        this.handleClose()
      } else {
        this.inputError = true
      }
    },
    resetForm() {
      this.inputError = false

      this.name = ''
      this.category = ''
      this.dueDate = ''
      this.priority = 'Medium'
    },
  },
}
</script>
