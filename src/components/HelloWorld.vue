<script setup lang="ts">
import type { catalogInterface } from '@/interfaces/catalog'

let props = defineProps<{
  msg: string[]
  list: catalogInterface[]
}>()
</script>

<template>
  <div class="greetings">
    <h1 class="green">Bookmarks</h1>
    <div class="catalogRow">
      <div v-for="(item, index) in props.list" :key="index">
        <div class="sizeGray" v-if="!item.bookmark">
          {{ item.title }}
        </div>
      </div>
    </div>
    <div v-for="(topic, index) in props.msg" :key="index">
      <h1 class="green">{{ topic }}</h1>
      <div class="catalogRow">
        <div v-for="(item, index) in props.list" :key="index">
          <div class="sizeGray" v-if="topic == item.topic">
            {{ item.title }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.sizeGray {
  width: 10rem;
  height: 10rem;
  background-color: gray;
  margin-bottom: 1rem;
  margin-right: 1rem;
}
.catalogRow {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
</style>
