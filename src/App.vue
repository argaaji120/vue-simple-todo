<template>
  <div id="app">
    <header>
      <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="45" height="45" />
      <h1>Todo List</h1>
    </header>

    <main>
      <!-- Add a new task -->
      <TaskInput v-on:add-task="addTask" />

      <div class="filter-category">
        <label for="categoryFilter">Filter by Category: </label>
        <select v-model="categoryFilter" id="categoryFilter">
          <option value="">All</option>
          <option>Work</option>
          <option>Personal</option>
        </select>
      </div>

      <!-- Show task list -->
      <TaskList
        v-if="tasks.length"
        v-bind:tasks="filteredTask"
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
  },
  computed: {
    filteredTask() {
      if (this.categoryFilter) {
        return this.tasks.filter((task) => task.category === this.categoryFilter)
      }

      return this.tasks
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
