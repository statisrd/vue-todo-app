<template>
  <div class="completed-tasks">
    <h2>Выполненные задачи</h2>
    <ul class="completed-tasks__list">
      <li class="completed-tasks__item" v-for="task in tasks" :key="task.id">
        {{ task.title }}
        <button class="btn" @click="$emit('delete-task', task.id)">Удалить</button>
      </li>
    </ul>
  </div>
</template>

  
  <script>
  export default {
    props: ['tasks'],
  };
  </script>
  

  