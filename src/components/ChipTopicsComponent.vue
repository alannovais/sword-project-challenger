<script setup lang="ts">
import { computed, ref } from 'vue'

const props = defineProps<{
  chipTopics: any
}>()

const emit = defineEmits(['selectTopics', 'unSelectTopics'])

const chipTopicsComponent = computed(() => {
  return props.chipTopics
})

let filterTopicsSelected: string[] = ref([])

const selectTopics = (topic: string) => {
  if (filterTopicsSelected.value.length == 0) filterTopicsSelected.value.push(topic)
  if (!filterTopicsSelected.value.some((e) => e == topic)) filterTopicsSelected.value.push(topic)
  emit('selectTopics', filterTopicsSelected)
}

const unSelectTopics = (topic: string) => {
  let filtering: string[] = []
  filtering = filterTopicsSelected.value.filter((e) => e != topic)
  filterTopicsSelected.value = filtering
  emit('unSelectTopics', filterTopicsSelected)
}
</script>

<template>
  <div class="chipClass">
    <div v-for="(item, index) in chipTopicsComponent" :key="index" class="space">
      <v-chip
        variant="elevated"
        v-if="filterTopicsSelected.includes(item)"
        @click="unSelectTopics(item)"
      >
        {{ item }}
      </v-chip>
      <v-chip v-if="!filterTopicsSelected.includes(item)" @click="selectTopics(item)">
        {{ item }}
      </v-chip>
    </div>
  </div>
</template>

<style scoped>
.chipClass {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.space {
  margin-right: 0.5rem;
}
</style>
