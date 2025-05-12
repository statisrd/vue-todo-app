<template>
  <li :class="['task-item', task.completed && 'task-item--done']">
    <input type="checkbox" :checked="task.completed" @change="$emit('toggle-complete', task.id)" />
    <span v-if="!editing" class="task-item__text" @dblclick="enableEdit">{{ task.title }}</span>
    <input
      v-else
      v-model="editedTitle"
      @keyup.enter="saveEdit"
      @keyup.esc="cancelEdit"
      @blur="saveEdit"
      ref="input"
      class="task-item__text"
    />
    <div class="task-item__buttons">
      <button class="btn" @click="enableEdit">Редактировать</button>
      <button class="btn" @click="$emit('delete-task', task.id)">Удалить</button>
    </div>
  </li>
</template>

<script>
export default {
  props: ['task'],
  data() {
    return {
      editing: false,
      editedTitle: this.task.title,
    };
  },
  methods: {
    enableEdit() {
      this.editing = true;
      this.editedTitle = this.task.title;
      this.$nextTick(() => {
        this.$refs.input.focus();
      });
    },
    saveEdit() {
      if (this.editedTitle.trim() && this.editedTitle !== this.task.title) {
        this.$emit('edit-task', { ...this.task, title: this.editedTitle.trim() });
      }
      this.editing = false;
    },
    cancelEdit() {
      this.editing = false;
      this.editedTitle = this.task.title;
    },
  },
};
</script>
