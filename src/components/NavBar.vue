<template>
  <nav>
    <router-link to="/">Home Page</router-link>
    <div class="nav-links">
      <router-link v-if="!isAuthenticated" to="/signin">Login</router-link>
      <router-link v-if="!isAuthenticated" to="/signup">Sign Up</router-link>
      <button v-if="isAuthenticated" @click="handleLogout">Logout</button>
    </div>
  </nav>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';

export default {
  name: "NavBar",
  data() {
    return {
      isMenuOpen: false
    };
  },
  computed: {
    ...mapGetters('auth', ['isAuthenticated']),
  },
  methods: {
    ...mapActions('auth', ['logout']),
    handleLogout() {
      this.logout();
    },
    toggleMenu() {
      this.isMenuOpen = !this.isMenuOpen;
    }
  }
};
</script>

<style scoped>
/* Navigation Bar Styling */
nav {
  width: 100%;
  max-width: 960px; 
  margin: 0 auto; 
  padding: 1rem 1.5rem; 
  background-color: #f4f4f4;
  color: #333;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1); 
  display: flex;
  justify-content: space-between;
  align-items: center;
}

/* Navigation Links Styling */
.nav-links {
  display: flex; 
  gap: 1.5rem;  
}

.nav-links a {
  text-decoration: none;
  color: #333;
  font-size: 1rem;
  transition: color 0.3s ease; 
}

.nav-links a:hover {
  color: #007bff; 
}

/* Logout Button Styling */
.nav-links button {
  background-color: #dc3545; 
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s ease;
}

.nav-links button:hover {
  background-color: #c82333;
}
</style>
