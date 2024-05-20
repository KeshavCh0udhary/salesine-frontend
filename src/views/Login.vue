<template>
  <div>
    <h3>Sign In</h3>
    <form @submit.prevent="submit">
      <input v-model="formData.email" type="email" placeholder="Email" />
      <input v-model="formData.password" type="password" placeholder="Password" />
      <button :disabled="isLoading" type="submit">{{ isLoading ? 'Signing In...' : 'Submit' }}</button>
    </form>
    <transition name="fade">
      <p v-if="signinError" class="error-message">{{ signinError }}</p>
    </transition>
  </div>
</template>

<script>
import { reactive, ref } from "vue"; // Import reactive and ref from vue
import { useRouter } from "vue-router";
import { useStore } from "vuex";

export default {
  name: "SignIn",
  setup() {
    const store = useStore();
    const router = useRouter();
    const formData = reactive({
      email: "",
      password: ""
    });
    const isLoading = ref(false); // Change to ref
    const signinError = ref('');

    const submit = async () => {
      isLoading.value = true; // Update isLoading
      try {
        const response = await fetch("http://localhost:3000/auth/login", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          credentials: 'include',
          body: JSON.stringify(formData)
        });
        const responseData = await response.json();
        if (response.ok) {
          const token = responseData.token;
          localStorage.setItem("user", token);
          store.dispatch('auth/login');
          router.push('/');
        } else {
          throw new Error(responseData.message || 'Signin failed');
        }
      } catch (error) {
        console.error('Signin error:', error.message);
        signinError.value = error.message;
      } finally {
        isLoading.value = false; // Update isLoading
      }
    };

    return {
      formData,
      isLoading,
      signinError,
      submit
    };
  }
};
</script>

<style scoped>
/* Sign In container styling */
div {
  max-width: 90%;
  margin: 20px auto;
  padding: 20px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.15);
}

/* Media Queries for Responsiveness */
@media (min-width: 576px) {
  div {
    max-width: 400px;
  }
}

/* Form Styling */
form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

h3 {
  font-size: 1.8rem;
  color: #333;
  margin-bottom: 1.5rem;
}

/* Input Fields */
input[type="email"],
input[type="password"] {
  width: 100%;
  padding: 0.8rem;
  font-size: 1rem;
  border: 1px solid #ced4da;
  border-radius: 4px;
  box-sizing: border-box;
}

/* Sign In Button */
button {
  width: 100%;
  padding: 0.8rem;
  font-size: 1rem;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #45a049;
}

input::placeholder {
  color: #999;
}

.error-message {
  color: #ff0000;
  margin-top: 10px;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
