<template>
  <div class="add-task-container">
    <h4 class="add-task-title">Add task</h4>
    <form @submit.prevent="onSubmit" class="add-task-form">
      <input type="text" v-model="name" placeholder="Add a task here" class="task-input" />
      <input type="submit" value="Submit" class="submit-button" />
    </form>
  </div>
</template>

<script>
import { mapActions } from 'vuex';
export default {
  name: 'AddTask',
  data() {
    return {
      name: '',
    };
  },
  methods: {
    ...mapActions(['addTasks']),
    async onSubmit() {
      let token = localStorage.getItem('user');
      if (!this.name.trim()) return;

      await this.addTasks({ name: this.name.trim(), token });
      this.name = '';
    }
  }
};
</script>

<style scoped>
/* AddTask Container */
.add-task-container {
  max-width: 500px;
  margin: 20px auto;
  padding: 30px;
  background-color: #2c3e50;
  border-radius: 15px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
  animation: fadeIn 1s ease-in-out;
  color: white;
}

/* AddTask Title */
.add-task-title {
  font-size: 28px;
  font-weight: 700;
  margin-bottom: 25px;
  text-align: center;
  color: #ecf0f1;
}

/* AddTask Form */
.add-task-form {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.5rem;
}

/* Task Input */
.task-input {
  width: 100%;
  padding: 15px 20px;
  font-size: 18px;
  border: none;
  border-radius: 5px;
  box-sizing: border-box;
  transition: box-shadow 0.3s ease, transform 0.3s ease;
  background-color: #34495e;
  color: #ecf0f1;
}

.task-input:focus {
  box-shadow: 0 0 15px rgba(52, 152, 219, 0.7);
  transform: translateY(-2px);
  outline: none;
}

/* Status Select */
.status-select {
  width: 100%;
  padding: 15px 20px;
  font-size: 18px;
  border: none;
  border-radius: 5px;
  box-sizing: border-box;
  background-color: #34495e;
  color: #ecf0f1;
  appearance: none;
  -webkit-appearance: none;
  transition: box-shadow 0.3s ease, transform 0.3s ease;
}

.status-select:focus {
  box-shadow: 0 0 15px rgba(52, 152, 219, 0.7);
  transform: translateY(-2px);
  outline: none;
}

/* Submit Button */
.submit-button {
  padding: 15px 25px;
  font-size: 18px;
  background-color: #3498db;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.submit-button:hover {
  background-color: #2980b9;
  transform: translateY(-2px);
}

/* Animation */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>