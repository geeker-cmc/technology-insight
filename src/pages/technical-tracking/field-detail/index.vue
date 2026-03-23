<script setup lang="ts">
import FieldIntro from "./components/FieldIntro.vue"
import FieldLists from "./components/FieldLists.vue"
</script>

<template>
  <div class="h-full flex flex-col bg-gray-50 overflow-hidden p-24px">
    <div class="max-w-1400px mx-auto w-full flex flex-col h-full min-h-0">
      <!-- 顶部介绍组件 -->
      <div class="shrink-0 mb-24px">
        <FieldIntro />
      </div>

      <!-- 下方的长列表三列布局组件 -->
      <div class="flex-1 min-h-0">
        <FieldLists />
      </div>
    </div>
  </div>
</template>

<style scoped>
</style>
