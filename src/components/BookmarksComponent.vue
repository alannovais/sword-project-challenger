<script setup lang="ts">
import { computed } from 'vue'
import type { catalogInterface } from '@/interfaces/catalog'

const props = defineProps<{
  arrayBookmarks: catalogInterface[]
}>()

const emit = defineEmits(['unCheckBookmaks'])

const arrayBookmarksComponent = computed(() => {
  return props.arrayBookmarks
})
const dlay: number = 50

const unCheckBookmaks = (catalog: catalogInterface) => {
  let filtering: string[] = []
  filtering = arrayBookmarksComponent.value.map((e) => {
    if (e.id == catalog.id) {
      e.bookmark = !e.bookmark
    }
    return e
  })
  arrayBookmarksComponent.value = filtering
  emit('unCheckBookmaks', filtering)
}
</script>

<template>
  <div class="container">
    <div>
      <h1>Bookmarks</h1>
    </div>
    <div class="catalogRow">
      <div
        v-for="(item, index) in arrayBookmarksComponent"
        :key="index"
        @click="unCheckBookmaks(item)"
      >
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
              <v-card-title>
                <div style="float: right">
                  <v-icon icon="mdi-star" />
                </div>
              </v-card-title>
              <v-card-text class="font-weight-medium mt-12 text-center text-subtitle-1">
                <div style="position: relative; top: -1.5rem">
                  <div>
                    {{ item.title }}
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
.container {
  display: flex;
  flex-direction: column;
}
.catalogRow {
  display: flex;
  flex-direction: row;
}
.space {
  margin-right: 0.5rem;
}
</style>
