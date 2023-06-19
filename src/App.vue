<script setup>
import { ref } from 'vue'
import AppLogin from '@/components/Login.vue'

const appUrl = 'http://localhost:8080/api/v1/'
const data = ref(null)
const error = ref(null)

fetch(`${appUrl}users/`, {
  headers: {
    Authorization: `Bearer ${localStorage.getItem('token')}`
  }
})
  .then((res) => res.json())
  .then((json) => (data.value = json))
  .catch((err) => (error.value = err))
</script>

<template>
    <div v-if="error">Oops! Error encountered: {{ error.message }}</div>
    <div v-else-if="data">
      Data loaded:
      <pre>{{ data }}</pre>
    </div>
    <div v-else>Loading...</div>
  <app-login />
</template>
