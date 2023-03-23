<script setup lang="ts">
import router from '@/router'
import { onMounted, reactive, ref, type Ref } from 'vue'
import type { userInterface } from '../interfaces/user'
import AlertOptionsComponent from '@/components/AlertOptionsComponent.vue'

let user: userInterface = reactive({
  login: '',
  password: '',
  active: false
})

let userMock: userInterface[] = [
  {
    login: '',
    password: '',
    active: false
  }
]

let openDialog: Ref<boolean> = ref(false)
let dialog = {
  titleDialog: 'Login failed!',
  msgDialog: 'User or Password can not be find in our database, please, check your data again',
  btOneDialog: 'Okay'
}

onMounted(() => {
  userMock = localStorage.getItem('user') != null && JSON.parse(localStorage.getItem('user'))
})

const singUp = (param: userInterface): any => {
  let checkAnyTrue = false
  if (param.login.trim().length == 0 || param.password.trim().length == 0) return (openDialog.value = true)
  if (userMock.length > 0) {
    let updateLocalStorage: userInterface[] = []
    userMock.forEach((element) => {
      if (param.login == element.login && param.password == element.password) {
        checkAnyTrue = true
        element.active = true
        updateLocalStorage.push(element)
      } else {
        updateLocalStorage.push(element)
      }
    })
    localStorage.setItem('user', JSON.stringify(updateLocalStorage))
    if (checkAnyTrue) return router.push({ path: '/dicovery' })
  }
  return (openDialog.value = true)
}

const signUp = (): void => {
  router.push({ path: '/signup' })
}

function closeDialog(emit: any): void {
  openDialog.value = emit
}
</script>

<template>
  <div class="space">&nbsp;</div>
  <div class="container">
    <form @keyup.enter="singUp(user)">
      <v-text-field v-model="user.login" variant="outlined" label="Username" />
      <v-text-field
        v-model="user.password"
        variant="outlined"
        label="Password"
        persistent-hint
        type="password"
      />
      <v-btn type="button" :width="'1000'" :color="'#343439'" @click="singUp(user)"
        ><span style="color: white"> Sign In</span>
      </v-btn>
    </form>
    <p>Don't have a account? <a @click="signUp"> Click here to Sign up.</a></p>
    <AlertOptionsComponent
      :set-dialog="openDialog"
      :dialogArray="dialog"
      @onCloseDialog="closeDialog"
    />
  </div>
</template>

<style scoped>
.space {
  min-height: 10rem;
}
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  flex-wrap: wrap;
}
a {
  text-decoration: underline;
}
</style>
