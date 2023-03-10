<script setup lang="ts">
import { computed } from 'vue'
const props = defineProps<{
  setDialog: boolean
  setTitle: string
  setMsg: string
  setBtnOne: string
  //setBtnTwo: string
}>()

const emit = defineEmits(['onCloseDialog'])

const dialog = computed(() => props.setDialog)
const title = computed(() => props.setTitle)
const msg = computed(() => props.setMsg)
const btnOne = computed(() => props.setBtnOne)
//const btnTwo = computed(() => props.setBtnTwo)
const onCloseDialog = () => {
  emit('onCloseDialog', false)
}
</script>

<template>
  <v-row justify="center">
    <v-dialog v-model="dialog" persistent width="auto">
      <v-card>
        <v-card-title class="text-h5"> {{ title }} </v-card-title>
        <v-card-text>{{ msg }}</v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="darken-1" variant="text" @click="onCloseDialog"> {{ btnOne }} </v-btn>
          <!-- <v-btn color="green-darken-1" variant="text" @click="dialog = false"> {{ btnTwo }} </v-btn> -->
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>
