<template>
  <div class="container mx-auto p-4">
    <h1 class="text-3xl font-bold mb-4">Task Manager</h1>
    <TaskForm @add-task="addTask" />
    <div v-if="errorMessage" class="text-red-500 mb-4">{{ errorMessage }}</div>
    <div class="mt-4">
      <TaskItem
        v-for="task in tasks"
        :key="task.id"
        :task="task"
        @mark-as-completed="markTaskAsCompleted"
      />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import TaskForm from './components/TaskForm.vue';
import TaskItem from './components/TaskItem.vue';

const API_URL = 'http://localhost:3000/tasks';

export default {
  name: 'App',
  components: { TaskForm, TaskItem },
  data() {
    return {
      tasks: [],
      errorMessage: '',
    };
  },
  mounted() {
    this.fetchTasks();
  },
  methods: {
    async fetchTasks() {
      try {
        const response = await axios.get(API_URL);
        this.tasks = response.data;
        this.errorMessage = '';
      } catch (error) {
        this.errorMessage = 'Failed to fetch tasks';
      }
    },
    async addTask(title, description) {
      try {
        await axios.post(API_URL, { title, description });
        this.fetchTasks(); // Refresh task list
        this.errorMessage = '';
      } catch (error) {
        this.errorMessage = 'Failed to add task';
      }
    },
    async markTaskAsCompleted(id) {
      try {
        await axios.patch(`${API_URL}/${id}`);
        this.fetchTasks(); // Refresh task list
        this.errorMessage = '';
      } catch (error) {
        this.errorMessage = error.response?.status === 404
          ? `Task with ID ${id} not found`
          : 'Failed to mark task as completed';
      }
    },
  },
};
</script>