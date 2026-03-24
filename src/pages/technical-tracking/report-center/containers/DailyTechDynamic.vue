<script setup lang="ts">
import { ref } from "vue"
import DailyTab from "../components/DailyTab.vue"
import ItemReviewTab from "../components/ItemReviewTab.vue"

const activeTab = ref("daily")
</script>

<template>
  <div class="h-full flex flex-col p-24px pb-0 bg-white">
    <el-tabs v-model="activeTab" class="w-full flex-1 flex flex-col daily-tabs">
      <!-- 日报 -->
      <el-tab-pane label="日报" name="daily" class="h-full flex flex-col">
        <DailyTab />
      </el-tab-pane>

      <!-- 条目审核 -->
      <el-tab-pane label="条目审核" name="review" class="h-full flex flex-col">
        <ItemReviewTab class="w-full flex-1" />
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<style scoped lang="scss">
:deep(.daily-tabs) {
  > .el-tabs__content {
    flex: 1;
    overflow: hidden;
  }
}
</style>
