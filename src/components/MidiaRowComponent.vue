<script setup lang="ts">
import { reactive } from 'vue'
import type { catalogInterface } from '@/interfaces/catalog'

const props = defineProps<{
  title: string
  orderDesc: boolean
  arrayGrouped: catalogInterface[]
}>()

const emit = defineEmits(['checkBookmaks'])

let arrayGrouped = reactive(props.arrayGrouped)

const dlay: number = 50

const checkBookmaks = (catalog: catalogInterface) => {
  emit('checkBookmaks', catalog)
}
const orderAsc = (a, b) => {
  let item1 = a.stars
  let item2 = b.stars
  return item1 < item2 ? -1 : item1 > item2 ? 1 : 0
}

const ordenarDesc = (a, b) => {
  let item1 = a.stars
  let item2 = b.stars
  return item1 > item2 ? -1 : item1 > item2 ? 1 : 0
}

const orderByAsc = () => {
  if (arrayGrouped.length == 0) return
  arrayGrouped.sort(orderAsc)
}

const orderByDesc = () => {
  if (arrayGrouped.length == 0) return
  arrayGrouped.sort(ordenarDesc)
}
</script>

<template>
  <div class="midiaClassColumn">
    <div class="midiaClassRow">
      <h1>{{ props.title }}</h1>
      <v-menu transition="slide-x-transition">
        <template v-slot:activator="{ props }">
          <v-icon
            color="primary"
            v-bind="props"
            icon="mdi-menu-down"
            :color="'#A1A1A4'"
            class="colorPositionArrow"
          ></v-icon>
        </template>
        <v-list>
          <v-list-item>
            <v-list-item-title @click="orderByAsc">Sort asc by stars</v-list-item-title>
            <v-list-item-title @click="orderByDesc">Sort desc by stars</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
    </div>
    <div class="midiaClassRow">
      <div v-for="(midia, indexMidia) in arrayGrouped" :key="indexMidia" class="spaceCards">
        <div class="space" @click="checkBookmaks(midia)">
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
              <v-card-title>
                <div style="float: right">
                  <v-icon icon="mdi-star-outline" />
                </div>
              </v-card-title>
              <v-card-text class="font-weight-medium mt-12 text-center text-subtitle-1">
                <div style="position: relative; top: -1.5rem">
                  <div>
                    {{ midia.title }}
                  </div>
                </div>
              </v-card-text>
            </v-card>
          </v-hover>
        </div>
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

.colorPositionArrow {
  position: relative;
  top: 0.6rem;
}
</style>
