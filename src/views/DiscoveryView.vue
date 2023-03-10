<script setup lang="ts">
import { ref, type Ref, reactive, onMounted, onBeforeMount } from 'vue'
import type { catalogInterface } from '@/interfaces/catalog'
import DiscoveryComponent from '../components/DiscoveryComponent.vue'
import NavBarComponent from '@/components/NavBarComponent.vue'
import json from '@/mock/catalogMoc.json'

let edit: Ref<boolean> = ref(false)
let listTask = json.data
let listTopic: string[] = reactive([])
let catalogModel: catalogInterface = {
  id: 0,
  title: '',
  bookmark: false,
  topic: '',
  stars: 0
}
let arrayFilter: catalogInterface[] = reactive([catalogModel])
onBeforeMount(() => {
  if (arrayFilter.find((e) => e.id == 0)) arrayFilter = listTask
  listOfTopics()
})
onMounted(() => {})

const updateList = (): catalogInterface[] => {
  let catalog: catalogInterface = {
    id: arrayFilter.length + 1,
    title: catalogModel.title,
    bookmark: false,
    topic: '',
    stars: 0
  }
  arrayFilter.push(catalog)
  return arrayFilter
}

const listOfTopics = (): string[] => {
  listTask.forEach((element) => {
    listTopic.length == 0 && listTopic.push(element.topic)
    if (!listTopic.includes(element.topic)) listTopic.push(element.topic)
  })
  return listTopic
}
</script>

<template>
  <div class="container">
    <div>
      <NavBarComponent :value="1" />
    </div>
    <div>
      <DiscoveryComponent :topics="listTopic" :list="arrayFilter" />
    </div>
  </div>
</template>

<style>
.container {
  display: flex;
  flex-direction: column;
}
.about {
  display: flex;
  flex-direction: row;
}
</style>
