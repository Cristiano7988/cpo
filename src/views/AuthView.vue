<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const email = ref('')
const password = ref('')
const error = ref('')

const auth = () => {
  error.value = ''

  if (!email.value) return (error.value = 'Insira um email')
  if (!/@/.test(email.value)) return (error.value = 'Formato de e-mail inválido')

  const [local, dominio] = email.value.split('@')
  if (!local) return (error.value = 'Formato de e-mail inválido')
  if (!dominio) return (error.value = 'Email sem domínio, insira um email válido')
  if (local.length <= 2 || dominio.length <= 2)
    return (error.value = 'Email muito curto, utilize outro email')

  if (password.value == '') return (error.value = 'Insira uma senha')
  if (password.value.length <= 8) return (error.value = 'Senha muito curta')

  localStorage.setItem('token', email.value + password.value) // Aqui salvamos o token na LocalStorage após resposta do backend, o mesmo token será validado validado na middleware das rotas protegidas
  location.pathname = '/about'
}
</script>

<template>
  <div class="auth">
    <h1>{{ router.currentRoute.value.name }}</h1>
    <input type="email" v-model="email" placeholder="Email" />
    <input type="password" v-model="password" placeholder="Password" />
    <button @click="auth">Vai Pokebola</button>
    <i v-if="error">{{ error }}</i>
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
i {
  color: firebrick;
}
</style>
