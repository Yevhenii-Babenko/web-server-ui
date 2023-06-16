<script>
import { ref } from 'vue'
export default {
  name: 'app-login',
  setup() {
    const isRegistered = ref(true)
    const loginUsername = ref('')
    const loginPassword = ref('')
    const registerUsername = ref('')
    const registerPassword = ref('')
    const registerEmail = ref('')
    const appUrl = 'http://localhost:8080/api/v1'

    //   TODO write down login functionality
    const login = async () => {
      try {
        // Make an HTTP request to your Java backend API for login
        const response = await fetch('http://localhost:8080/api/v1/login/', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            username: loginUsername.value,
            password: loginPassword.value
          })
        })

        if (response.ok) {
          // Login successful, perform necessary actions (e.g., store token, navigate to another page)
          console.log('Login successful')
        } else {
          // Login failed, display error message to the user
          console.log('Login failed')
        }
      } catch (error) {
        console.error(error)
      }
    }
    const register = async () => {
      try {
        const response = fetch('http://localhost:8080/api/v1/register/', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            username: registerUsername.value,
            password: registerPassword.value,
            email: registerEmail.value,
            createdOn: new Date().toISOString(),
            lastLogin: new Date().toISOString()
          })
        })
        if (response.ok) {
          // Registration successful, update the UI accordingly
          isRegistered.value = true
        } else {
          // Registration failed, display error message to the user
          console.log('Registration failed')
        }
      } catch (e) {
        console.error(e.message)
      }
    }
    const toggleRegistration = () => {
      isRegistered.value = !isRegistered.value
    }
    return {
      isRegistered,
      loginUsername,
      loginPassword,
      registerUsername,
      registerPassword,
      login,
      register,
      toggleRegistration,
      registerEmail
    }
  }
}
</script>

<template>
  <div>
    <h1>Login</h1>
    <form v-if="!isRegistered" @submit.prevent="login">
      <div>
        <label for="username">Username:</label>
        <input type="text" id="username" v-model="loginUsername" />
      </div>
      <div>
        <label for="password">Password:</label>
        <input type="password" id="password" v-model="loginPassword" />
      </div>
      <button type="submit">Login</button>
    </form>
    <form v-else @submit.prevent="register">
      <div>
        <label for="username">Username:</label>
        <input type="text" id="username" v-model="registerUsername" />
      </div>
      <div>
        <label for="password">Password:</label>
        <input type="password" id="password" v-model="registerPassword" />
      </div>
      <div>
        <label for="password">Email:</label>
        <input type="email" id="email" v-model="registerEmail" />
      </div>
      <button type="submit">Register</button>
    </form>
    <div v-if="isRegistered">
      <p>Registration successful! You can now log in.</p>
      <button @click="toggleRegistration">Back to Login</button>
    </div>
  </div>
</template>

<style scoped></style>
