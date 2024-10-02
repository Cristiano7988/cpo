<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
const router = useRouter()
const email = ref('')
const password = ref('')

const auth = () => {
  localStorage.setItem('token', email.value + password.value)
  location.pathname = '/about'
}
</script>

<template>
  <div class="auth">
    <h1>{{ router.currentRoute.value.name }}</h1>
    <input type="email" v-model="email" placeholder="Email" />
    <input type="password" v-model="password" placeholder="Password" />
    <button @click="auth">Vai Pokebola</button>
  </div>
</template>

<style scoped>
.auth {
  display: flex;
  flex-direction: column;
  gap: 5px;
  max-width: 400px;
  margin: auto;
}
h1 {
  text-transform: capitalize;
}
</style>
