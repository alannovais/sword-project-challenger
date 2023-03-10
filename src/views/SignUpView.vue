<script setup lang="ts">
import NavBarComponent from '@/components/NavBarComponent.vue'
import { reactive, ref } from 'vue'
import type { userInterface } from '@/interfaces/user'
import router from '@/router'
import AlertOptionsComponent from '@/components/AlertOptionsComponent.vue'

let user: userInterface = reactive({
  login: '',
  password: '',
  active: false
})

let openDialog: boolean = ref(false)
let titleDialog: string = 'Duplicated!'
let msgDialog: string =
  'Sorry but this username has been used for other person, could you be change?'
let btOneDialog: string = 'Okay'

const register = (param: userInterface) => {
  let userLocalstorage: userInterface[] = []
  let arrayToUpdate: userInterface[] = []
  userLocalstorage = JSON.parse(localStorage.getItem('user'))

  if (userLocalstorage != null) {
    if (userLocalstorage.some((e) => e.login == param.login)) return (openDialog.value = true)
    arrayToUpdate = userLocalstorage.concat([
      {
        login: param.login,
        password: param.password,
        active: true,
        email: param.email
      }
    ])
    localStorage.setItem('user', JSON.stringify(arrayToUpdate))
    return router.push({ path: '/dicovery' })
  } else {
    localStorage.setItem(
      'user',
      JSON.stringify([
        {
          login: param.login,
          password: param.password,
          active: true,
          email: param.email
        }
      ])
    )
    return router.push({ path: '/dicovery' })
  }
}

function closeDialog(emit: any): void {
  openDialog.value = emit
}
</script>

<template>
  <NavBarComponent :value="2" />
  <main>
    <div style="width: 40rem; position: relative; left: -12rem; z-index: 9999">
      <form @keyup.enter="register(user)">
        <v-text-field v-model="user.login" variant="outlined" label="Username *" required />
        <v-text-field
          v-model="user.password"
          label="Password *"
          persistent-hint
          type="password"
          required
          variant="outlined"
        />
        <v-text-field v-model="user.email" label="Email" variant="outlined" required />
        <v-btn type="button" :width="'1000'" :color="'#343439'" @click="register(user)"
          ><span style="color: white">Save</span>
        </v-btn>
      </form>
    </div>
    <AlertOptionsComponent
      :set-dialog="openDialog"
      :set-title="titleDialog"
      :set-msg="msgDialog"
      :set-btn-one="btOneDialog"
      @onCloseDialog="closeDialog"
    />
  </main>
</template>

<style>
#app {
    display: flex;
}
</style>
