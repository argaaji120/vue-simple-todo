<template>
  <ul>
    <TaskItem
      v-for="task in tasks"
      v-bind:key="task.id"
      v-bind:task="task"
      v-on:complete="$emit('completeTask', task.id)"
      v-on:delete="$emit('deleteTask', task.id)"
      v-on:edit="(editedTask) => $emit('editTask', editedTask)"
    />
  </ul>
</template>

<script>
import TaskItem from './TaskItem.vue'

export default {
  props: ['tasks'],
  components: { TaskItem },
}
</script>
