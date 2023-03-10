<script setup lang="ts">
import type { catalogInterface } from '@/interfaces/catalog'
import { computed, ref, type Ref } from 'vue'

let props = defineProps<{
  topics: string[]
  list: catalogInterface[]
}>()
let dlay: number = 50
let arrayEmpity: catalogInterface[] = []
let arrayBookmarks: catalogInterface[] = ref([])
let filterTopicsSelected: string[] = ref([])
let orderDesc: boolean = ref(false)

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

const checkBookmaks = (catalog: catalogInterface): void => {
  catalog.bookmark = !catalog.bookmark
  arrayEmpity.length == 0 && arrayEmpity.push(catalog)
  if (catalog.bookmark && !arrayEmpity.some((e) => e.id == catalog.id)) arrayEmpity.push(catalog)
  arrayBookmarks.value = arrayEmpity.filter((e) => e.bookmark == true)
}

const unCheckBookmaks = (catalog: catalogInterface): string[] => {
  let filtering: string[] = []
  filtering = arrayBookmarks.value.map((e) => {
    if (e.id == catalog.id) {
      e.bookmark = !e.bookmark
    }
    return e
  })
  arrayBookmarks.value = filtering
  return arrayBookmarks
}

const selectTopics = (topic: string): string[] => {
  if (filterTopicsSelected.value.length == 0) filterTopicsSelected.value.push(topic)
  if (!filterTopicsSelected.value.some((e) => e == topic)) filterTopicsSelected.value.push(topic)
  return filterTopicsSelected
}

const unSelectTopics = (topic: string): string[] => {
  let filtering: string[] = []
  filtering = filterTopicsSelected.value.filter((e) => e != topic)
  filterTopicsSelected.value = filtering
  return filterTopicsSelected
}

const filterTopics = (list: catalogInterface[], topic: string): catalogInterface[] => {
  console.log(orderDesc.value)
  return list.filter((e) => e.topic == topic)
}

// const sortByStarsAsc = () => {
//   if (arrayList.length == 0) return
//   arrayList.sort((a, b) => a.stars + b.stars)
//   console.log(arrayList)
// }
</script>

<template>
  <div class="displayView">
    <div class="container">
      <div>
        <h1>Bookmarks</h1>
        <v-icon icon="mdi-star-outline" />
      </div>
      <div class="catalogRow">
        <div v-for="(item, index) in arrayBookmarks" :key="index" @click="unCheckBookmaks(item)">
          <div class="space">
            <v-hover v-slot="{ isHovering, props }" :open-delay="dlay" v-if="item.bookmark">
              <v-card
                :elevation="isHovering ? 16 : 2"
                :class="{ 'on-hover': isHovering }"
                class="mx-auto"
                height="150"
                width="250"
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
      <div v-for="(item, index) in chipTopics" :key="index">
        <div>
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
    </div>

    <div v-for="(topic, index) in arrayTopics" :key="index">
      <div>
        <h1>{{ topic }}</h1>
      </div>
      <div class="catalogRow">
        <div v-for="(item, index) in filterTopics(arrayList, topic)" :key="index">
          <div class="space" @click="checkBookmaks(item)">
            <v-hover v-slot="{ isHovering, props }" :open-delay="dlay">
              <v-card
                :elevation="isHovering ? 16 : 2"
                :class="{ 'on-hover': isHovering }"
                class="mx-auto"
                height="150"
                width="250"
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
  margin-bottom: 1rem;
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
