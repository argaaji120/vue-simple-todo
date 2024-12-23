<template>
  <div id="app">
    <header>
      <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="75" height="75" />
      <h1>Todo List</h1>
    </header>

    <main>
      <!-- Add a new task -->
      <TaskInput v-on:add-task="addTask" />

      <TaskList
        v-if="tasks.length"
        v-bind:tasks="tasks"
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
    }
  },
  methods: {
    addTask(taskName) {
      this.tasks.push({
        id: this.tasks.length ? this.tasks[this.tasks.length - 1].id + 1 : 1,
        name: taskName,
        completed: false,
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
