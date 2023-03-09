<script setup lang="ts">
import type { catalogInterface } from '@/interfaces/catalog'
import { reactive } from 'vue'

let props = defineProps<{
  msg: string[]
  list: catalogInterface[]
}>()
let dlay: number = 50
let arrayBookmarks: catalogInterface[] = reactive([])
let arrayEmpity: catalogInterface[] = reactive([])

const filter = (list: catalogInterface[], topic: string): catalogInterface[] => {
  return list.filter((e) => e.topic == topic)
}

const checkBookmaks = (list: catalogInterface): catalogInterface[] => {
  list.bookmark = !list.bookmark
  list.bookmark && arrayBookmarks.push(list)
  arrayEmpity = arrayBookmarks.filter((e) => e.bookmark == true)
  return arrayEmpity
}
let filterTopicsSelected: string[] = []
let teste: any[] = []

const selectTopics = (topic: string): string[] => {
  if (filterTopicsSelected.length == 0) teste.push(topic)
  if (filterTopicsSelected.some((e) => e == topic))
    teste = filterTopicsSelected.filter((e) => e != topic)
  if (filterTopicsSelected.length == 0) filterTopicsSelected.push(topic)
  if (!filterTopicsSelected.some((e) => e == topic)) filterTopicsSelected.push(topic)

  if (teste.length == 0) filterTopicsSelected = teste
  console.log(filterTopicsSelected)

  return filterTopicsSelected
}

const sortByStarsAsc = () => {
  if (props.list.length == 0) return
  props.list.sort((a, b) => a.stars + b.stars)
  console.log(props.list)
}

const sortByStarsDesc = () => {
  if (props.list.length == 0) return
  props.list.sort((a, b) => a.stars - b.stars)
  console.log(props.list)
}
</script>

<template>
  <div class="displayView">
    <div class="container">
      <div>
        <h1>Bookmarks</h1>
      </div>
      <div class="catalogRow">
        <div v-for="(item, index) in arrayBookmarks" :key="index">
          <div class="space">
            <v-hover v-slot="{ isHovering, props }" :open-delay="dlay" v-if="item.bookmark">
              <v-card
                :elevation="isHovering ? 16 : 2"
                :class="{ 'on-hover': isHovering }"
                class="mx-auto"
                height="150"
                width="150"
                :color="'#A1A1A4'"
                v-bind="props"
              >
                <v-card-text class="font-weight-medium mt-12 text-center text-subtitle-1">
                  {{ item.title }}
                </v-card-text>
              </v-card>
            </v-hover>
          </div>
        </div>
      </div>
    </div>
    <div class="chip">
      <div v-for="(item, index) in props.msg" :key="index" @click="selectTopics(item)">
        <div>
          <v-chip variant="elevated" v-if="filterTopicsSelected.find((e) => e == item)">
            {{ item }}
          </v-chip>
          <v-chip v-esle> {{ item }} </v-chip>
        </div>
      </div>
    </div>

    <div v-for="(topic, index) in props.msg" :key="index">
      <div>
        <h1>{{ topic }}</h1>
      </div>
      <div class="catalogRow">
        <div v-for="(item, index) in filter(props.list, topic)" :key="index">
          <div class="space" @click="checkBookmaks(item)">
            <v-hover v-slot="{ isHovering, props }" :open-delay="dlay">
              <v-card
                :elevation="isHovering ? 16 : 2"
                :class="{ 'on-hover': isHovering }"
                class="mx-auto"
                height="150"
                width="150"
                :color="'#A1A1A4'"
                v-bind="props"
              >
                <v-card-text class="font-weight-medium mt-12 text-center text-subtitle-1">
                  {{ item.title }}
                </v-card-text>
              </v-card>
            </v-hover>
          </div>
        </div>
      </div>
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
}
.sizeGray {
  width: 10rem;
  height: 10rem;
  background-color: gray;
  margin-bottom: 1rem;
  margin-right: 1rem;
}
.catalogRow {
  display: grid;
  grid-gap: 1px;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
}
.space {
  margin-right: 0.3rem;
  margin-bottom: 0.3rem;
}
.chip {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}
</style>
