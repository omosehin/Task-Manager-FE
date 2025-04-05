<template>
  <form @submit.prevent="submitTask" class="mb-4 p-4 bg-gray-100 rounded shadow">
    <h2 class="text-lg font-semibold">Add New Task</h2>
    <p class="text-sm text-gray-600">Fill in the details below to create a new task.</p>
    <div class="flex flex-col space-y-4">
      <input
        v-model="title"
        type="text"
        placeholder="Task Title"
        class="border p-4 rounded"
        required
      />
      <textarea
        v-model="description"
        placeholder="Task Description"
        class="border p-2 rounded"
        required
        rows="5"
      ></textarea>
      <button type="submit" class="bg-blue-500 text-white p-2 rounded hover:bg-blue-600">
        Add Task
      </button>
    </div>
  </form>
</template>
  
<script>
  export default {
    name: 'TaskForm',
    data() {
      return {
        title: '',
        description: '',
      };
    },
    methods: {
      submitTask() {
        if (this.title && this.description) {
          this.$emit('add-task', this.title, this.description);
          this.title = '';
          this.description = '';
        }
      },
    },
  };
</script>