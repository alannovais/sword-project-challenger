<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
  setDialog: boolean,
  dialogArray: object
}>()
const emit = defineEmits(['onCloseDialog'])

const dialogOpen = computed(() => props.setDialog)
const dialog = computed(() => props.dialogArray)

const onCloseDialog = () => {
  emit('onCloseDialog', false)
}
</script>

<template>
  <v-row justify="center">
    <v-dialog v-model="dialogOpen" persistent width="auto">
      <v-card>
        <v-card-title class="text-h5"> {{ dialog.titleDialog }} </v-card-title>
        <v-card-text>{{ dialog.msgDialog }}</v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="darken-1" variant="text" @click="onCloseDialog"> {{ dialog.btOneDialog }} </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>
