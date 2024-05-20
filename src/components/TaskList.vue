<template>
  <div class="task-container">
    <div v-for="task in tasks" :key="task.id" :class="{ 'hide-task': shouldHideTask(task) }">
      <div @click="updateTask(task._id)" class="task-item" :class="{ 'completed': type === 'completed' && task.status }">
        {{ task.name }}
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions } from "vuex";

export default {
  props: {
    tasks: {
      type: Array,
      required: true,
    },
    type: {
      type: String,
      required: true,
    },
  },
  methods: {
    ...mapActions(["updateTasks"]),
    updateTask(taskId) {
      const token = localStorage.getItem("user");
      this.updateTasks({ taskId, token });
    },
    shouldHideTask(task) {
      return (this.type === "completed" && !task.status) || (this.type === "not-completed" && task.status);
    },
  },
};
</script>

<style scoped>
/* Task Container */
.task-container {
  display: flex;
  flex-direction: column;
}

/* Task Item */
.task-item {
  padding: 1rem;
  border-radius: 8px;
  margin-bottom: 1rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.task-item:hover {
  background-color: #f8f9fa;
  transform: translateY(-3px);
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
}

.completed {
  color: #6bd24b; /* Green color for completed tasks */
}

/* Hide Task Class */
.hide-task {
  display: none;
}
</style>