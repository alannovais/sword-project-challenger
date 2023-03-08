<script setup lang="ts">
import router from '@/router';
import { onMounted } from 'vue';
import type { userInterface } from '../interfaces/user'

let user: userInterface = { 
  login: '', 
  password: '',
  active: false
}

onMounted(() => {
})

function singUp(param: userInterface) {
  if (param.login.length == 0) return
  if (param.password.length == 0) return

  return router.push({ path: '/dicovery' })
}
</script>

<template>
  <main class="container">
    <input type="text" v-model="user.login" placeholder="Login" />
    <input type="text" v-model="user.password" placeholder="Password" />
    <button @click="singUp(user)">Entrar</button>
  </main>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
}
</style>
