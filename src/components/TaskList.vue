<template>
  <ul>
    <task-item
      v-for="task in tasks"
      :key="task.id"
      :task="task"
      @delete-task="$emit('delete-task', $event)"
      @toggle-complete="$emit('toggle-complete', $event)"
      @edit-task="$emit('edit-task', $event)"
    />
  </ul>
</template>

<script>
import TaskItem from './TaskItem.vue';

export default {
  props: ['tasks'],
  components: { TaskItem },
};
</script>
