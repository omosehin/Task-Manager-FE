<template>
  <div
    class="border p-4 mb-2 rounded flex justify-between items-center"
    :class="{ 'bg-gray-200': task.completed }"
  >
    <div>
      <h3 :class="{ 'line-through': task.completed }" class="font-semibold">
        {{ task.title }}
      </h3>
      <p :class="{ 'line-through': task.completed }">{{ task.description }}</p>
    </div>
    <button
      @click="markAsCompleted"
      class="bg-green-500 text-white p-2 rounded hover:bg-green-600"
      :disabled="task.completed"
    >
      {{ task.completed ? 'Completed' : 'Mark as Completed' }}
    </button>
  </div>
</template>
  
<script>
  export default {
    name: 'TaskItem',
    props: {
      task: {
        type: Object,
        required: true,
      },
    },
    methods: {
      markAsCompleted() {
        if (!this.task.completed) {
          this.$emit('mark-as-completed', this.task.id);
        }
      },
    },
  };
</script>