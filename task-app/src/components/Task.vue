<template>
  <div class="container">
    <div class="task">
      <!-- title -->
      <div class="title">
        <h1>Task App</h1>
      </div>
      <!-- form -->
      <div class="form">
        <input type="text" placeholder="New Task" v-model="newTask"  @keyup.enter="addTask" />
        <button @click = "addTask"><i class="fas fa-plus"></i></button>
      </div>
      <!-- task lists -->
      <div class="taskItems">
        <ul>
          <task-item v-bind:task="task" v-for="(task) in filteredTasks" :key="task.id" @remove="removeTask" @complete="completeTask(task)"></task-item>
        </ul>
      </div>
      <!-- buttons -->
      <div class="clearBtns">
        <button @click="filterCompleted = null">All</button>
        <button @click="filterCompleted = false">Pending</button>
        <button @click="filterCompleted = true">Completed</button>
      </div>
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
  components:{
    TaskItem
  },
  data(){
    return {
      newTask: "",
      filterCompleted: null,
    }
  },
  computed:{
    inComplete(){
      return this.tasks.filter(this.inProgress).length;
    },
    filteredTasks() {
      if (this.filterCompleted === null) {
        return this.tasks;
      } else {
        return this.tasks.filter((task) => task.completed === this.filterCompleted);
      }
    },
  },
  methods: {
    addTask() {
    if (this.newTask.trim() !== "") { 
      console.log(this.filterCompleted);
      const updatedTasks = [
        ...this.tasks,
        { title: this.newTask, completed: false }
      ];
      this.$emit("update-tasks", updatedTasks);
      this.newTask = ""; 
    }
  },
    inProgress(task){
      return !this.isCompleted(task);
    },
    isCompleted(task){
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
    completeTask(task){
      task.completed = !task.completed;
    }
  },
};
</script>