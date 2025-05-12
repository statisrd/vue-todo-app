<template>
  <div class="app">
    <h1>TODO List</h1>
    <task-form @add-task="addTask" />

    <div class="filters">
      <button class="filters__button btn" @click="setFilter('all')">Все</button>
      <button class="filters__button btn" @click="setFilter('active')">Активные</button>
      <button class="filters__button btn" @click="setFilter('completed')">Выполненные</button>
    </div>

    <div class="task-layout">
      <div class="task-block">
        <task-list
          :tasks="filteredTasks"
          @delete-task="deleteTask"
          @toggle-complete="toggleComplete"
          @edit-task="editTask"
        />
      </div>
      <!-- Блок с выполненными задачами -->
      <div class="task-block task-block--completed">
        <completed-tasks
          :tasks="completedTasks"
          @delete-task="deleteTask"
          @toggle-complete="toggleComplete"
        />
      </div>
    </div>
  </div>
</template>

<script>
import TaskForm from './components/TaskForm.vue';
import TaskList from './components/TaskList.vue';
import CompletedTasks from './components/CompletedTasks.vue';

export default {
  components: { TaskForm, TaskList, CompletedTasks },

  data() {
    return {
      tasks: [],
      filter: 'all',
    };
  },

  computed: {
    filteredTasks() {
      return this.filterTasks(this.filter);
    },

    // Массив выполненных задач для отдельного блока
    completedTasks() {
      return this.tasks.filter(task => task.completed);
    },
  },

  methods: {
    setFilter(newFilter) {
      this.filter = newFilter;
    },

    filterTasks(filter) {
      if (filter === 'completed') {
        return this.tasks.filter(task => task.completed);
      } else if (filter === 'active') {
        return this.tasks.filter(task => !task.completed);
      }
      return this.tasks;
    },

    async fetchTasks() {
      const localData = localStorage.getItem('tasks');
      if (localData) {
        this.tasks = JSON.parse(localData);
      } else {
        const response = await fetch('https://jsonplaceholder.typicode.com/todos?_limit=10');
        const data = await response.json();
        this.tasks = data.map(t => ({ id: t.id, title: t.title, completed: t.completed }));
        this.saveTasks();
      }
    },

    addTask(title) {
      const newTask = {
        id: Date.now(),
        title,
        completed: false,
      };
      this.tasks.push(newTask);
    },

    deleteTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id);
    },

    toggleComplete(id) {
      const task = this.tasks.find(t => t.id === id);
      task.completed = !task.completed;
    },

    editTask(updatedTask) {
      const index = this.tasks.findIndex(t => t.id === updatedTask.id);
      this.tasks[index] = updatedTask;
    },

    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
  },

  watch: {
    tasks() {
      this.saveTasks();
    },
  },

  mounted() {
    this.fetchTasks();
  },
};
</script>
