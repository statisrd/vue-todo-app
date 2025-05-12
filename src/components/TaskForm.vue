<template>
  <form class="task-form" @submit.prevent="submitTask">
    <input v-model="title" class="task-form__input" placeholder="Новая задача" required />
    <button type="submit" class="task-form__button">Добавить</button>
  </form>
</template>

<script>
export default {
  data() {
    return { title: '' };
  },
  methods: {
    submitTask() {
      this.$emit('add-task', this.title);
      this.title = ''; 
    },
  },
};
</script>
