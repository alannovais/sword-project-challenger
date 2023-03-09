<script setup lang="ts">
import router from '@/router'
import { onMounted, reactive } from 'vue'
import type { userInterface } from '../interfaces/user'

let user: userInterface = reactive({
  login: '',
  password: '',
  active: false
})

onMounted(() => {})

const singUp = (param: userInterface): any => {
  if (param.login.length == 0) return
  if (param.password.length == 0) return

  return router.push({ path: '/dicovery' })
}

const signUp = (): any => {
  return router.push({ path: '/dicovery' })
}

</script>

<template>
  <main class="container">
    <v-text-field v-model="user.login" label="Login"></v-text-field>
    <v-text-field v-model="user.password" label="Password"></v-text-field>
    <v-btn :color="'#343439'" @click="singUp(user)"
      ><span style="color: white"> Sign In</span>
    </v-btn>
    <p>Don't have a account? <a @click="signUp"> Click here to Sign up.</a></p>
  </main>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  width: 20rem;
}
a {
  text-decoration: underline;
}
</style>
