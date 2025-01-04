<template>
  <div id="app">
    <div class="container">
      <nav class="navbar border-bottom mb-3">
        <a class="navbar-brand" href="javascript:void(0)">
          <img
            src="./assets/logo.svg"
            alt="Vue Logo"
            width="45"
            height="45"
            class="img-fluid d-inline-block align-text-bottom mx-auto"
          />
          <span class="h1"> Todo List</span>
        </a>
      </nav>

      <main>
        <!-- Add a new task -->
        <TaskInput v-on:add-task="addTask" />

        <!-- Filter by Category -->
        <div class="filter-category mt-3 mb-2">
          <label for="categoryFilter">Filter by Category: </label>
          <select v-model="categoryFilter" id="categoryFilter">
            <option value="">All</option>
            <option>Work</option>
            <option>Personal</option>
          </select>
        </div>

        <!-- Filter by Priority -->
        <div class="mb-2">
          <label for="priorityFilter">Filter by Priority: </label>
          <select v-model="priorityFilter" id="priorityFilter">
            <option value="">All</option>
            <option>High</option>
            <option>Medium</option>
            <option>Low</option>
          </select>
        </div>

        <!-- Sort by Due Date -->
        <div class="sort-order mb-3">
          <label for="sortOrder">Sort by Due Date: </label>
          <select v-model="sortOrder" id="sortOrder">
            <option value="asc">Ascending</option>
            <option value="desc">Descending</option>
          </select>
        </div>

        <!-- Show task list -->
        <TaskList
          v-if="tasks.length"
          v-bind:tasks="sortedTasks"
          v-on:complete-task="completeTask"
          v-on:edit-task="editTask"
          v-on:delete-task="deleteTask"
        />

        <!-- Task Editing Modal -->
        <TaskEdit
          v-if="editingTask"
          v-bind:task="editingTask"
          v-on:edited-task="updateTask"
          v-on:cancel-editing="cancelEdit"
        />
      </main>
    </div>
  </div>
</template>

<script>
import TaskInput from './components/TaskInput.vue'
import TaskList from './components/TaskList.vue'
import TaskEdit from './components/TaskEdit.vue'

export default {
  components: { TaskInput, TaskList, TaskEdit },
  data() {
    return {
      tasks: [],
      editingTask: null,
      categoryFilter: '',
      priorityFilter: '',
      sortOrder: 'asc',
    }
  },
  methods: {
    addTask(task) {
      let max = { id: 0 }

      if (this.tasks.length) {
        max = this.tasks.reduce((prev, current) => {
          return prev && prev.id > current.id ? prev : current
        })
      }

      this.tasks.push({ id: max.id + 1, ...task })
    },
    completeTask(taskId) {
      this.tasks = this.tasks.map((task) =>
        task.id === taskId ? { ...task, completed: !task.completed } : task,
      )
    },
    updateTask(editedTask) {
      this.tasks = this.tasks.map((task) => (task.id === editedTask.id ? { ...editedTask } : task))
      this.editingTask = null
    },
    deleteTask(taskId) {
      this.tasks = this.tasks.filter((task) => task.id !== taskId)
    },
    filteredTask() {
      let filtered = this.tasks

      if (this.categoryFilter) {
        filtered = this.tasks.filter((task) => task.category === this.categoryFilter)
      }

      if (this.priorityFilter) {
        filtered = this.tasks.filter((task) => task.priority === this.priorityFilter)
      }

      if (this.categoryFilter && this.priorityFilter) {
        filtered = this.tasks.filter(
          (task) => task.category === this.categoryFilter && task.priority === this.priorityFilter,
        )
      }

      return filtered
    },
    editTask(task) {
      this.editingTask = { ...task }
    },
    cancelEdit() {
      this.editingTask = null
    },
  },
  computed: {
    sortedTasks() {
      const tasksToSort = this.filteredTask()

      return tasksToSort.sort((a, b) => {
        const dateA = new Date(a.dueDate)
        const dateB = new Date(b.dueDate)
        return this.sortOrder === 'asc' ? dateA - dateB : dateB - dateA
      })
    },
  },
  mounted() {
    const savedTasks = localStorage.getItem('tasks')

    if (savedTasks) this.tasks = JSON.parse(savedTasks)
  },
  watch: {
    tasks: {
      handler(newTasks) {
        localStorage.setItem('tasks', JSON.stringify(newTasks))
      },
      deep: true,
    },
  },
}
</script>

<style scoped>
/*  */
</style>
