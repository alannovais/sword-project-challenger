<script setup lang="ts">
import { onMounted } from 'vue';
const props = defineProps<{
  value: number
}>()
let value: number = 1

onMounted(() => {
  value = props.value
})
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
            <v-btn> LOGO </v-btn>
          </div>
          <div>
            <v-btn> Discovery </v-btn>
          </div>
          <div>&nbsp;</div>
          <div>&nbsp;</div>
          <div>&nbsp;</div>
          <div>
            <v-btn> Username </v-btn>
          </div>

          <div>
            <v-btn> Logout </v-btn>
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
  background-color: #a1a1a4;
}

.v-application__wrap {
  min-height: 6rem;
}
</style>
