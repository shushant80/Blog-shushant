<template>
  <div class="auth-container">
    <div v-if="isSignUp">
      <h1>Sign Up</h1>
      <form @submit.prevent="signUp">
        <div class="form-group">
          <label for="newUserId">User ID</label>
          <input type="text" v-model="newUserId" id="newUserId" required />
        </div>
        <div class="form-group">
          <label for="newPassword">Password</label>
          <input type="password" v-model="newPassword" id="newPassword" required />
        </div>
        <button type="submit">Sign Up</button>
        <button type="button" @click="toggleAuth">Already have an account? Login</button>
      </form>
    </div>
    <div v-else>
      <h1>Login</h1>
      <form @submit.prevent="loginWithCustomCredentials">
        <div class="form-group">
          <label for="userId">User ID</label>
          <input type="text" v-model="userId" id="userId" required />
        </div>
        <div class="form-group">
          <label for="password">Password</label>
          <input type="password" v-model="password" id="password" required />
        </div>
        <button type="submit">Login</button>
        <button type="button" @click="toggleAuth">Don't have an account? Sign Up</button>
      </form>
      <button @click="loginWithGoogle">Login with Google</button>
    </div>
  </div>
</template>

<script>
import { getAuth, signInWithPopup, GoogleAuthProvider } from 'firebase/auth';

export default {
  name: 'AuthPage',
  data() {
    return {
      userId: '',
      password: '',
      newUserId: '',
      newPassword: '',
      isSignUp: false // Toggle between login and sign-up forms
    };
  },
  methods: {
    loginWithGoogle() {
      const provider = new GoogleAuthProvider();
      const auth = getAuth();
      signInWithPopup(auth, provider)
        .then(() => {
          this.$router.push('/');
        })
        .catch((error) => {
          console.error('Error logging in with Google:', error);
        });
    },
    loginWithCustomCredentials() {
      const customUserId = 'customUser';
      const customPassword = 'customPassword';

      if (this.userId === customUserId && this.password === customPassword) {
        localStorage.setItem('authenticated', 'true');
        this.$router.push('/');
      } else {
        alert('Invalid User ID or Password');
      }
    },
    signUp() {
      // Implement sign-up logic here
      console.log('User ID:', this.newUserId);
      console.log('Password:', this.newPassword);
      // For now, we'll just log the user data to the console
      // In a real application, you would save this data to a database
      this.isSignUp = false; // Switch back to login form after sign-up
    },
    toggleAuth() {
      this.isSignUp = !this.isSignUp; // Toggle between login and sign-up
    }
  }
};
</script>

<style scoped>
.auth-container {
  max-width: 400px;
  margin: auto;
  padding: 1em;
  margin-top: 20%;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.form-group {
  margin-bottom: 1em;
}

.form-group label {
  display: block;
  margin-bottom: 0.5em;
}

.form-group input {
  width: 100%;
  padding: 0.5em;
  box-sizing: border-box;
}

button {
  width: 100%;
  padding: 10px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-top: 10px;
}

button:hover {
  background-color: #0056b3;
}
</style>
