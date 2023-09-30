<template>
    <li>
      <div class="task-item">
        <select v-model="selectedOption" @change="handleChange" class="custom-dropdown">
          <option
            v-for="(option, index) in options"
            :key="index"
            :value="option.value"
          >
            {{ option.label }}
          </option>
        </select>
        <button @click="removeTask"><i class="far fa-trash-alt"></i></button>
        <span :class="className">{{ task.title }}</span>
        <div v-if="showInput">
          <input
            type="text"
            v-model="newOption"
            @keyup.enter="addNewOption"
            @blur="addNewOption"
          />
        </div>
      </div>
    </li>
  </template>
  
  <script>
  export default {
    name: "TaskItem",
    props: {
      task: Object,
      filterOptions: Array,
    },
    data() {
      return {
        showInput: false,
        newOption: "",
        // Set the initial value based on the task's completion status
        selectedOption: this.task.completed ? "Completed" : "Pending",
      };
    },
    computed: {
      options() {
        const customStatus = "Add New";
        // Include "Add New" option in the dropdown if it's not already there
        if (this.filterOptions.indexOf(customStatus) === -1) {
          return [...this.filterOptions, customStatus].map((value) => ({
            label: value,
            value,
          }));
        }
        return this.filterOptions.map((value) => ({
          label: value,
          value,
        }));
      },
    },
    methods: {
      handleChange() {
        if (this.selectedOption === "Add New") {
          this.showInput = true;
        } else {
          this.showInput = false;
          if (this.selectedOption === "Custom") {
            // Emit the new status to the parent component (task-component.vue)
            this.$emit("new-status", this.newOption);
          } else {
            // Emit the selected status to the parent component (task-component.vue)
            this.$emit("update-status", {
              ...this.task,
              completed: this.selectedOption === "Completed",
            });
          }
        }
      },
      addNewOption() {
  if (this.newOption.trim() !== "") {
    // Emit the new status to the parent component (task-component.vue)
    this.$emit("new-status", this.newOption);

    // Reset the input field and hide it
    this.newOption = "";
    this.showInput = false;
  }
}
    },
  };
  </script>
  