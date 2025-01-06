<template>
  <MyModal ref="modal" modal-id="editTaskModal" title="Edit Task" v-on:close="hideModal">
    <template #body>
      <div v-if="inputError" class="text-danger mb-3">* Please fill out all fields.</div>

      <div class="mb-3">
        <label for="editTask" class="form-label">Task</label>
        <input
          class="form-control"
          type="text"
          id="editTask"
          placeholder="Task"
          v-model="editingTask.name"
        />
      </div>

      <div class="mb-3">
        <label for="editCategory">Category</label>
        <select class="form-select" id="editCategory" v-model="editingTask.category">
          <option disabled value="">Select Category</option>
          <option>Work</option>
          <option>Personal</option>
        </select>
      </div>

      <div class="mb-3">
        <label for="editDueDate">Due Date</label>
        <input class="form-control" type="date" id="editDueDate" v-model="editingTask.dueDate" />
      </div>

      <div class="mb-3">
        <label for="editPriority">Priority</label>
        <select class="form-select" id="editPriority" v-model="editingTask.priority">
          <option value="High">High</option>
          <option value="Medium">Medium</option>
          <option value="Low">Low</option>
        </select>
      </div>
    </template>

    <template #footer>
      <button class="btn btn-success" v-on:click="editTask">Update</button>
    </template>
  </MyModal>
</template>

<script>
import MyModal from './MyModal.vue'

export default {
  components: { MyModal },
  props: {
    task: {
      type: Object,
      required: false,
    },
  },
  emits: ['cancelEditing', 'editedTask'],
  data() {
    return {
      editingTask: this.task,
      inputError: false,
    }
  },
  watch: {
    task: {
      immediate: true,
      handler(newTask) {
        this.editingTask = newTask ? { ...newTask } : null
      },
    },
  },
  methods: {
    hideModal() {
      this.$refs.modal.closeModal()
    },
    showModal() {
      this.inputError = false
      this.$refs.modal.openModal()
    },
    editTask() {
      if (this.editingTask.name.trim() && this.editingTask.dueDate) {
        this.$emit('editedTask', this.editingTask)
        this.hideModal()

        this.inputError = false
      } else {
        this.inputError = true
      }
    },
  },
}
</script>
