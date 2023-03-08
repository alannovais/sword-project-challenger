<script setup lang="ts">
import { ref, type Ref, reactive, onMounted, onBeforeMount } from 'vue'
import HelloWorld from '../components/HelloWorld.vue'
import type { catalogInterface } from '@/interfaces/catalog'
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
  console.log({ arrayFilter })
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

const filterTopic = (): catalogInterface[] => {
  if (listTask.length == 0) return []
  arrayFilter = listTask.filter((e) => {
    if (e.topic == 'Javascript') return e
  })
  console.log(arrayFilter)
  return arrayFilter
}

const sortByStarsAsc = () => {
  if (listTask.length == 0) return
  listTask.sort((a, b) => a.stars + b.stars)
  console.log(listTask)
}

const sortByStarsDesc = () => {
  if (listTask.length == 0) return
  listTask.sort((a, b) => a.stars - b.stars)
  console.log(listTask)
}

const bookmarkCheck = () => {
  if (listTask.length == 0) return
  listTask.map((e, index) => {
    if (index == 0) e.bookmark = !e.bookmark
  })
  console.log(listTask)
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
    <div class="about">
      <NavBarComponent />
    </div>
    <div>
      <input type="text" name="" id="" placeholder="Name Task" v-model="catalogModel.title" />
      <button @click="filterTopic()">sort topic</button>
      <button @click="sortByStarsAsc()">sort Stars</button>
      <button @click="sortByStarsDesc()">sort Stars Desc</button>
      <button @click="bookmarkCheck()">bookmark check</button>
      <button v-if="edit">Editar</button>
      <button v-else @click="updateList()">Save</button>
    </div>
    <div>
      <HelloWorld :msg="listTopic" :list="arrayFilter" />
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
