<script setup lang="ts">
import { computed, ref } from "vue"
import { useRouter } from "vue-router"

import Archived from "./containers/Archived.vue"
import DailyTechDynamic from "./containers/DailyTechDynamic.vue"
import DraftBox from "./containers/DraftBox.vue"
import StrategicWeekly from "./containers/StrategicWeekly.vue"

const activeMenu = ref("daily")

const reportCategories = [
  {
    title: "报告类型",
    children: [
      { name: "每日科技动态", key: "daily", component: DailyTechDynamic },
      { name: "战略信息周报", key: "weekly", component: StrategicWeekly }
    ]
  },
  {
    title: "生产状态",
    children: [
      { name: "草稿箱", key: "draft", component: DraftBox },
      { name: "已归档", key: "archived", component: Archived }
    ]
  }
]

const currentComponent = computed(() => {
  for (const group of reportCategories) {
    const found = group.children.find(item => item.key === activeMenu.value)
    if (found) {
      return found.component
    }
  }
  return DailyTechDynamic
})
</script>

<template>
  <div class="app-container flex h-full w-full overflow-hidden bg-[var(--v3-body-bg-color)] p-16px gap-16px">
    <!-- 左侧栏 -->
    <aside class="w-240px flex-shrink-0 bg-white rounded-8px overflow-y-auto shadow-sm">
      <div class="p-16px">
        <div v-for="(group, index) in reportCategories" :key="index" class="mb-24px">
          <!-- 分组标题 -->
          <div class="text-[#999999] text-14px font-bold mb-16px">
            {{ group.title }}
          </div>

          <!-- 分类项列表 -->
          <div class="space-y-4px">
            <div
              v-for="item in group.children"
              :key="item.key"
              @click="activeMenu = item.key"
              class="relative flex items-center justify-between px-16px py-12px rounded-8px cursor-pointer transition-colors"
              :class="[
                activeMenu === item.key ? 'bg-[#E3F2FD] text-[#1976D2]' : 'text-[#333333]',
              ]"
            >
              <!-- 激活态左侧指示条 -->
              <div v-if="activeMenu === item.key" class="absolute left-0 top-10px bottom-10px w-4px bg-[#1976D2] rounded-r-4px" />

              <div class="text-16px font-medium">
                {{ item.name }}
              </div>
            </div>
          </div>

          <!-- 分隔线 -->
          <div v-if="index === 0" class="mt-24px h-1px bg-[#F5F5F5] w-full" />
        </div>
      </div>
    </aside>
    <main class="flex-1 min-w-0 overflow-hidden">
      <div class="h-full w-full flex flex-col rounded-12px shadow-sm border border-gray-100 overflow-hidden border-solid">
        <component :is="currentComponent" />
      </div>
    </main>
  </div>
</template>

<style scoped lang="scss">
/* 隐藏各列的滚动条但保留滚动功能 */
.overflow-y-auto {
  -ms-overflow-style: none; /* IE and Edge */
  scrollbar-width: none; /* Firefox */

  &::-webkit-scrollbar {
    display: none; /* Chrome, Safari and Opera */
  }
}
</style>
