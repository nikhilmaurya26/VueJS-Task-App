<template>
  <div id="app">
    <task v-bind:tasks="tasks" @clear-all-tasks="clearAllTasks" @update-tasks="updateTasks"></task>
  </div>
</template>

<script>
import Task from "./components/Task";

export default {
  name: "App",
  components: {
    Task,
  },
  data() {
    return {
      tasks: [
        {
          id: 1,
          title: "Nikhil Task",
          completed: true,
        },
        {
          id: 2,
          title: "Watch netflix",
          completed: true,
        },
        {
          id: 3,
          title: "Go shopping",
          completed: false,
        },
        {
          id: 4,
          title: "Task 4",
          completed: false,
        },
        {
          id: 5,
          title: "Send email",
          completed: false,
        },
      ],
    };
  },
  methods: {
    clearAllTasks() {
      this.tasks = [];
    },
    updateTasks(updatedTasks) {
      
      this.tasks = updatedTasks;
    },
  },
};
</script>