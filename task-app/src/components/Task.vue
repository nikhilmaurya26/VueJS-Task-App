<template>
  <div class="container">
    <div class="task">
      <!-- title -->
      <div class="title">
        <h1>Task App</h1>
      </div>
      <!-- form -->
      <div class="form">
        <input
          type="text"
          placeholder="New Task"
          v-model="newTask"
          @keyup.enter="addTask"
        />
        <button @click="addTask"><i class="fas fa-plus"></i></button>
      </div>
      <!-- Filter dropdown -->
      <div class="filterDropdown custom-filter-dropdown">
  <select v-model="selectedFilter" @change="filterTasks">
    <option v-for="option in filterOptions" :key="option" :value="option">
      {{ option }}
    </option>
  </select>
</div>
      <!-- task lists -->
      <div class="taskItems">
        <ul>
          <task-item
            v-for="(task, index) in filteredByStatus"
            :key="task.id"
            :task="task"
            @remove-task="removeTask(index)"
            @update-status="updateTaskStatus"
            @new-status="addCustomStatus"
            :filterOptions="filterOptions"
          ></task-item>
        </ul>
      </div>
      <!-- buttons -->
      <div class="clearBtns">
        <button @click="clearCompleted">Clear completed</button>
        <button @click="clearAll">Clear all</button>
      </div>
      <!-- pending task -->
      <div class="pendingTasks">
        <span>Pending Tasks: {{ inComplete }}</span>
      </div>
    </div>
  </div>
</template>

<script>
import TaskItem from "./Task-item";

export default {
  name: "task-component",
  props: {
    tasks: Array,
  },
  components: {
    TaskItem,
  },
  data() {
    return {
      newTask: "",
      selectedFilter: "All",
      filterOptions: ["All", "Completed", "Pending"],
    };
  },
  computed: {
    inComplete() {
      return this.tasks.filter(this.inProgress).length;
    },
    filteredByStatus() {
      if (this.selectedFilter === "All") {
        return this.tasks;
      } else if (this.selectedFilter === "Completed") {
        return this.tasks.filter((task) => task.completed);
      } else if (this.selectedFilter === "Pending") {
        return this.tasks.filter((task) => !task.completed);
      } else {
        // Handle custom statuses here
        return this.tasks.filter((task) => task.title === this.selectedFilter);
      }
    },
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== "") {
        const updatedTasks = [
          ...this.tasks,
          { title: this.newTask, completed: false },
        ];
        this.$emit("update-tasks", updatedTasks);
        this.newTask = "";
      }
    },
    inProgress(task) {
      return !this.isCompleted(task);
    },
    isCompleted(task) {
      return task.completed;
    },
    clearCompleted() {
      const updatedTasks = this.tasks.filter(this.inProgress);
      this.$emit("update-tasks", updatedTasks);
    },
    clearAll() {
      this.$emit("clear-all-tasks");
    },
    removeTask(index) {
      const updatedTasks = [...this.tasks];
      updatedTasks.splice(index, 1);
      this.$emit("update-tasks", updatedTasks);
    },
    updateTaskStatus(updatedTask) {
      // Find and update the task in the task list
      const updatedTasks = this.tasks.map((task) => {
        if (task.id === updatedTask.id) {
          return updatedTask;
        }
        return task;
      });
      this.$emit("update-tasks", updatedTasks);
    },
    addCustomStatus(newStatus) {
      // Check if the new status is not in filterOptions and add it
      if (!this.filterOptions.includes(newStatus)) {
        this.filterOptions.splice(this.filterOptions.length - 1, 0, newStatus);
      }
    },
    filterTasks() {
  // Only update selectedFilter for standard filter options, not for custom statuses
  if (this.filterOptions.includes(this.selectedFilter)) {
    // No need to assign this.selectedFilter to itself
    // this.selectedFilter = this.selectedFilter;
    this.$emit("update-filter", this.selectedFilter);
  }
},
  },
};
</script>
