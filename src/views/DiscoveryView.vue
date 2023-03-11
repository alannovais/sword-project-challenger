<script setup lang="ts">
import { reactive, onBeforeMount } from 'vue'
import type { catalogInterface } from '@/interfaces/catalog'
import DiscoveryComponent from '../components/DiscoveryComponent.vue'
import NavBarComponent from '@/components/NavBarComponent.vue'
import json from '@/mock/catalogMoc.json'

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
</style>
