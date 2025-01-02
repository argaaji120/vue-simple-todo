<template>
  <div class="overlay-modal"></div>

  <div class="modal">
    <h2>Edit Task</h2>
    <input v-model="editingTask.name" placeholder="Task name" />

    <select v-model="editingTask.category">
      <option>Work</option>
      <option>Personal</option>
    </select>

    <input v-model="editingTask.dueDate" type="date" />

    <select v-model="editingTask.priority">
      <option value="High">High</option>
      <option value="Medium">Medium</option>
      <option value="Low">Low</option>
    </select>

    <br />

    <button v-on:click="editTask">Update</button>
    <button v-on:click="$emit('cancelEditing')">Cancel</button>
  </div>
</template>

<script>
export default {
  props: ['task'],
  emits: ['cancelEditing', 'editedTask'],
  data() {
    return {
      editingTask: this.task,
    }
  },
  methods: {
    editTask() {
      if (this.editingTask.name.trim() && this.editingTask.dueDate) {
        this.$emit('editedTask', this.editingTask)
      }
    },
  },
}
</script>

<style scoped>
input,
select {
  margin-right: 7px;
}

button {
  margin-right: 5px;
  margin-top: 7px;
  float: right;
}

.modal {
  position: fixed;
  top: 30%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  z-index: 999;
}

.overlay-modal {
  display: block;
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  z-index: 999;
  background: rgba(0, 0, 0, 0.5);
}
</style>
