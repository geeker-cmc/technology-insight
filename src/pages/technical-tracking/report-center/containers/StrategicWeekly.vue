<script setup lang="ts">
import { ref } from "vue"
import WeeklyItemReviewTab from "../components/WeeklyItemReviewTab.vue"
import WeeklyTab from "../components/WeeklyTab.vue"

const activeTab = ref("weekly")
</script>

<template>
  <div class="h-full flex flex-col p-24px pb-0 bg-white">
    <el-tabs v-model="activeTab" class="w-full flex-1 flex flex-col daily-tabs">
      <!-- 周报 -->
      <el-tab-pane label="周报" name="weekly" class="h-full flex flex-col">
        <WeeklyTab />
      </el-tab-pane>

      <!-- 条目审核 -->
      <el-tab-pane label="条目审核" name="review" class="h-full flex flex-col">
        <WeeklyItemReviewTab class="w-full flex-1" />
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
