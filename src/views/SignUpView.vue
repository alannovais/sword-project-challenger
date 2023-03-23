<script setup lang="ts">
import NavBarComponent from '@/components/NavBarComponent.vue'
import { reactive, ref, type Ref } from 'vue'
import type { userInterface } from '@/interfaces/user'
import router from '@/router'
import AlertOptionsComponent from '@/components/AlertOptionsComponent.vue'

let user: userInterface = reactive({
  login: '',
  password: '',
  active: false
})

let openDialog: Ref<boolean> = ref(false)
let openDialogMiss: Ref<boolean> = ref(false)

let dialog = {
  titleDialog: 'Duplicated!',
  msgDialog: 'Sorry but this username has been used for other person, could you be change?',
  btOneDialog: 'Okay'
}

let dialogMiss = {
  titleDialog: 'Please, fill the fields',
  msgDialog: 'Hey, you leaved login/password in blank, fill the fields for create access',
  btOneDialog: 'Okay'
}

const register = (param: userInterface) => {
  if (param.login.trim().length == 0 || param.password.trim().length == 0) return
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

const closeDialog = (emit: any): void => {
  openDialog.value = emit
}

const closeDialogMiss = (emit: any): void => {
  openDialog.value = emit
}
</script>

<template>
  <NavBarComponent :value="2" />
  <div class="singup spaceVh">
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
    :dialogArray="dialog"
    @onCloseDialog="closeDialog"
  />
  <AlertOptionsComponent
    :set-dialog="openDialogMiss"
    :dialogArray="dialogMiss"
    @onCloseDialog="closeDialogMiss"
  />
</template>

<style>
#app {
  display: flex;
}
.singup {
  display: flex;
  justify-content: center;
  align-items: center;
}
.spaceVh {
  position: relative;
  top: -10vh
}
</style>
