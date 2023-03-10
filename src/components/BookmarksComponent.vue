<script setup lang="ts">
let arrayBookmarks: catalogInterface[] = ref([])
let dlay: number = 50
</script>

<template>
  <div>
    <h1>Bookmarks</h1>
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
</template>

<style></style>
