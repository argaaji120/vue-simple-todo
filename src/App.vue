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
    </main>
  </div>
</template>

<script>
import TaskInput from './components/TaskInput.vue'
import TaskList from './components/TaskList.vue'

export default {
  components: { TaskInput, TaskList },
  data() {
    return {
      tasks: [],
      categoryFilter: '',
      priorityFilter: '',
      sortOrder: 'asc',
    }
  },
  methods: {
    addTask(task) {
      this.tasks.push({
        ...task,
        id: this.tasks.length ? this.tasks[this.tasks.length - 1].id + 1 : 1,
      })
    },
    completeTask(taskId) {
      this.tasks = this.tasks.map((task) =>
        task.id === taskId ? { ...task, completed: !task.completed } : task,
      )
    },
    editTask(editedTask) {
      this.tasks = this.tasks.map((task) =>
        task.id === editedTask.id ? { ...task, name: editedTask.editedName } : task,
      )
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
@import url('https://fonts.googleapis.com/css2?family=Poppins&display=swap');

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
