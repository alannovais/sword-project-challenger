<script setup lang="ts">
import { onMounted } from 'vue'
import router from '@/router'
import type { userInterface } from '@/interfaces/user'
const props = defineProps<{
  value: number
}>()
let value: number = 1

onMounted(() => {
  value = props.value
})

const logout = () => {
  let arrayToUpdate: userInterface[] = []
  let newArray: userInterface[] = []
  arrayToUpdate = JSON.parse(localStorage.getItem('user'))
  if (arrayToUpdate == null) return router.push({ path: '/' })
  arrayToUpdate.forEach((element) => {
    if (element.active == true) element.active = !element.active
    newArray.push(element)
  })
  localStorage.setItem('user', JSON.stringify(newArray))
  return router.push({ path: '/' })
}

const username = () => {
  return router.push({ path: '/signup' })
}
</script>

<template>
  <v-app>
    <v-app-bar title="My site" app>
      <v-bottom-navigation grow v-model="value" active>
        <div
          style="
            display: flex;
            flex: 1;
            flex-direction: row;
            justify-content: space-between;
            align-items: center;
          "
        >
          <div>
            <v-btn @click="logout"> LOGO </v-btn>
          </div>
          <div>
            <v-btn> Discovery </v-btn>
          </div>
          <div>&nbsp;</div>
          <div>&nbsp;</div>
          <div>&nbsp;</div>
          <div>
            <v-btn @click="username"> Username </v-btn>
          </div>

          <div>
            <v-btn @click="logout"> Logout </v-btn>
          </div>
        </div>
      </v-bottom-navigation>
    </v-app-bar>
  </v-app>
</template>

<style>
.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

.colorNavBar {
  background-color: #A1A1A4;
}
.v-application__wrap {
  min-height: 8vh;
}
</style>
