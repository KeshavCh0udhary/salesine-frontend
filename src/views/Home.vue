<template>
  <div>
    <h3>Salesine Task Manager</h3>
    <AddTask v-if="isAuthenticated" />
    <div id="tasks" v-if="isAuthenticated">
      <h4>Pending Tasks</h4>
      <RenderTask :tasks="completedTask" type="not-completed" />
      <h4>Completed</h4>
      <RenderTask :tasks="completedTask" type="completed" />
    </div>
    <div v-else>
      Authenticate Yourself to access application
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';
import RenderTask from '../components/TaskList.vue';
import AddTask from '../components/TaskForm.vue';
import VueJwtDecode from "vue-jwt-decode";

export default {

  name: 'TaskTracker',
  computed: {
    ...mapGetters(['allTasks']),
    ...mapGetters('auth', ['isAuthenticated']),
    completedTask() {
      return this.allTasks;
    },
    // checkuser(){
    //   let token = localStorage.getItem("user");
    //   if(token!=null)
    //     return true
    //   else  
    //     return false
    // }
  },
  methods: {
    ...mapActions('auth', ['login']),
    ...mapActions(['fetchTasks']),
  },

  mounted() {
    this.login();
    let token = localStorage.getItem("user");
    if (token != null) {
      try {
        let decoded = VueJwtDecode.decode(token);
        this.user = decoded;
        this.fetchTasks(token);
      } catch (error) {
        console.log(error, 'error from decoding token');
      }
    }
  },
  components: {
    RenderTask,
    AddTask
  },
};
</script>

<style scoped>
/* Home container styling */
div {
  max-width: 90%;
  margin: 20px auto;
  padding: 20px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.15);
  text-align: left;
}

/* Media Queries for Responsiveness */
@media (min-width: 768px) {
  div {
    max-width: 800px;
    text-align: center;
  }
}

/* Title Styling */
h3 {
  font-size: 1.8rem;
  color: #293462;
  margin-bottom: 1.5rem;
}

/* TaskTracker Container */
#tasks-container {
  margin-top: 1.5rem;
}

/* AddTask Form */
#add-task-form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

@media (min-width: 576px) {

  /* Adjust breakpoint as needed */
  #add-task-form {
    flex-direction: row;
    align-items: center;
  }
}

#task-input {
  padding: 0.8rem;
  font-size: 1rem;
  border: 1px solid #ced4da;
  border-radius: 5px;
}

#add-button {
  padding: 0.8rem 1.2rem;
  font-size: 1rem;
  background-color: #35654d;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

#add-button:hover {
  background-color: #284939;
}

/* RenderTask (Tasks List) */
#tasks {
  margin-top: 1.5rem;
  list-style: none;
  padding: 0;
}

h4 {
  color: #333;
  font-size: 1.4rem;
  margin-top: 2rem;
}
</style>
