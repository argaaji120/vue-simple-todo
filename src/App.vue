<template>
  <div id="app">
    <header>
      <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="45" height="45" />
      <h1>Todo List</h1>
    </header>

    <main>
      <!-- Add a new task -->
      <TaskInput v-on:add-task="addTask" />

      <!-- Filter by Category -->
      <div class="filter-category">
        <label for="categoryFilter">Filter by Category: </label>
        <select v-model="categoryFilter" id="categoryFilter">
          <option value="">All</option>
          <option>Work</option>
          <option>Personal</option>
        </select>
      </div>

      <!-- Filter by Priority -->
      <div>
        <label for="priorityFilter">Filter by Priority: </label>
        <select v-model="priorityFilter" id="priorityFilter">
          <option value="">All</option>
          <option>High</option>
          <option>Medium</option>
          <option>Low</option>
        </select>
      </div>

      <!-- Sort by Due Date -->
      <div class="sort-order">
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
#app {
  margin-left: 32px;
  font-family: 'Poppins', serif;
  font-weight: 400;
  font-style: normal;
}

header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

.filter-category {
  margin-top: 10px;
}

.sort-order {
  margin-top: 5px;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 1rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
