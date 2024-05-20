<template>
  <div class="signup-container">
    <h3>Sign Up</h3>
    <form @submit.prevent="submit" class="signup-form">
      <input v-model="formData.name" type="text" placeholder="Username" required />
      <input v-model="formData.email" type="email" placeholder="Email" required />
      <input v-model="formData.password" type="password" placeholder="Password" required />
      <button :disabled="isLoading || isSignupSuccess" type="submit">
        {{ isLoading ? 'Signing Up...' : (isSignupSuccess ? 'Signup Success' : 'Submit') }}
      </button>
    </form>
    <transition name="fade">
      <p v-if="signupError" class="error-message">{{ signupError }}</p>
    </transition>
  </div>
</template>

<script>
export default {
  name: "SignUpForm",
  data() {
    return {
      formData: {
        name: '',
        email: '',
        password: ''
      },
      isLoading: false,
      signupError: '',
      isSignupSuccess: false
    };
  },
  methods: {
    async submit() {
      this.isLoading = true;
      try {
        const response = await fetch('http://localhost:3000/auth/signup', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(this.formData)
        });
        const responseData = await response.json();
        console.log(response)
        if (response.ok) {
          // Signup successful
          console.log('Signup successful:', responseData);
          this.isSignupSuccess = true;
          this.resetForm();
        } else {
          responseData.message = "User already exists"
          throw new Error(responseData.message);
        }
      } catch (error) {
        console.error('Signup error:', error.message);
        this.signupError = error.message;
      } finally {
        this.isLoading = false;
      }
    },
    resetForm() {
      this.formData.name = '';
      this.formData.email = '';
      this.formData.password = '';
      this.signupError = '';
      this.isSignupSuccess = false;
    }
  }
};
</script>

<style scoped>
.signup-container {
  max-width: 400px;
  margin: 50px auto;
  padding: 20px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.signup-form {
  display: flex;
  flex-direction: column;
  align-items: center;
}

h3 {
  font-size: 24px;
  color: #333;
  margin-bottom: 20px;
}

input {
  padding: 10px;
  font-size: 16px;
  margin-bottom: 15px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  width: 100%;
  padding: 0.8rem 1.2rem;
  font-size: 1rem;
  background-color: #4caf50;
  color: white;
  border: 2px solid transparent;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease, border-color 0.3s ease;
}

button:hover {
  background-color: #45a049;
  border-color: white;
}

.error-message {
  color: #ff0000;
  margin-top: 10px;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>