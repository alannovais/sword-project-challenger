<script setup lang="ts">
import { computed, ref } from 'vue'
import type { catalogInterface } from '@/interfaces/catalog'
import BookmarksComponent from './BookmarksComponent.vue'
import ChipTopicsComponent from './ChipTopicsComponent.vue'
import MidiaComponent from './MidiaComponent.vue'

let props = defineProps<{
  topics: string[]
  list: catalogInterface[]
}>()
let arrayBookmarks: catalogInterface[] = ref([])
let filterTopicsSelected: string[] = ref([])

const arrayList = computed(() => {
  return props.list
})

const chipTopics = computed(() => {
  return props.topics
})

const arrayTopics = computed(() => {
  let array = []
  array = chipTopics.value.filter((e) => {
    if (!filterTopicsSelected.value.includes(e)) return e
  })
  return array
})

const checkBookmaks = (emit: any): void => {
  console.log(emit)

  arrayBookmarks.value = emit
}

const unCheckBookmaks = (emit: any): void => {
  return (arrayBookmarks.value = emit)
}

const selectTopics = (emit: any): string[] => {
  return (filterTopicsSelected.value = emit.value)
}

const unSelectTopics = (emit: any): string[] => {
  return (filterTopicsSelected.value = emit.value)
}
</script>

<template>
  <div class="displayView">
    <div class="container">
      <BookmarksComponent :array-bookmarks="arrayBookmarks" @unCheckBookmaks="unCheckBookmaks" />
    </div>
    <div>
      <ChipTopicsComponent
        :chip-topics="chipTopics"
        @select-topics="selectTopics"
        @un-select-topics="unSelectTopics"
      />
    </div>
    <div class="catalogRow">
      <MidiaComponent
        :arrayList="arrayList"
        @checkBookmaks="checkBookmaks"
        :arrayTopics="arrayTopics"
        :filterTopicsSelected="filterTopicsSelected"
      />
    </div>
  </div>
</template>

<style scoped>
.displayView {
  display: flex;
  flex-direction: column;
}
.container {
  display: flex;
  flex-direction: column;
  margin-bottom: 1rem;
}
.space {
  margin-right: 0.3rem;
  margin-bottom: 0.3rem;
}
</style>
