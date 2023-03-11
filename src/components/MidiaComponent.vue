<script setup lang="ts">
import { computed, ref, onMounted, type Ref } from 'vue'
import type { catalogInterface } from '@/interfaces/catalog'
import MidiaRowComponent from './MidiaRowComponent.vue'

const props = defineProps<{
  arrayTopics: string[]
  arrayList: catalogInterface[]
  filterTopicsSelected: string[]
}>()

const emit2 = defineEmits(['checkBookmaks'])

const arrayTopicComponent = computed(() => {
  return props.arrayTopics
})
const arrayListComponent = computed(() => {
  return props.arrayList
})

const filtredChip = computed(() => {
  return props.filterTopicsSelected
})

let orderDesc: Ref<boolean> = ref(false)

let arrayEmpity: catalogInterface[] = []

const checkBookmaks = (emit: any) => {
  emit.bookmark = !emit.bookmark
  arrayEmpity.length == 0 && arrayEmpity.push(emit)
  if (emit.bookmark && !arrayEmpity.some((e) => e.id == emit.id)) arrayEmpity.push(emit)
  emit2(
    'checkBookmaks',
    arrayEmpity.filter((e) => e.bookmark == true)
  )
}

let groupedArray = ref([])

function groupBy(array, key) {
  return array.reduce((hash, obj) => {
    if (obj[key] === undefined) return hash
    return Object.assign(hash, { [obj[key]]: (hash[obj[key]] || []).concat(obj) })
  }, {})
}

onMounted(() => {
  groupedArray.value = groupBy(arrayListComponent.value, 'topic')
})

function showChip(topic: string): boolean {
  if(filtredChip.value.length == 0) return true
  return filtredChip.value.some((e: string) => e != topic)
}
</script>

<template>
  <div class="midiaClassColumn">
    <div v-for="(arrayGrouped, indexGroup) in groupedArray" :key="indexGroup">
      <div class="midiaClassRow" v-if="showChip(indexGroup.toString())">
        <MidiaRowComponent
          :title="indexGroup.toString()"
          :orderDesc="orderDesc"
          :arrayGrouped="arrayGrouped"
          @checkBookmaks="checkBookmaks"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
.midiaClassColumn {
  display: flex;
  flex-direction: column;
}
.midiaClassRow {
  display: flex;
  flex-direction: row;
}

.spaceCards {
  margin-right: 0.5rem;
  margin-bottom: 0.5rem;
}
</style>
