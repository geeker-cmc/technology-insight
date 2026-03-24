<script setup lang="ts">
import { Delete, Edit, Search, User } from "@element-plus/icons-vue"
import { computed, ref } from "vue"

// 搜索和筛选状态
const searchQuery = ref("")
const selectedType = ref("all")
const dateRange = ref<[string, string] | null>(null)

// 模拟归档数据
const mockList = [
  {
    id: 1,
    title: "战略科研信息周报（第9期）",
    summary: "专注于新能源与储能领域的专题信息，重点收录储能政策、光伏风电装机、电网改造等内容，适用于能源电力业务部门分发。",
    author: "张三",
    publishTime: "2026-03-02",
    type: "weekly"
  },
  {
    id: 2,
    title: "每日科技动态（第74期）",
    summary: "跟踪当日重大科技政策、前沿技术突破和产业动态，包含人工智能、可控核聚变等领域的最新进展及深度分析。",
    author: "李四",
    publishTime: "2026-03-20",
    type: "daily"
  },
  {
    id: 3,
    title: "战略科研信息周报（第8期）",
    summary: "聚焦集成电路及半导体制造工艺，汇总本周关键政策和核心企业动态，分析新一代集成电路装备产业化趋势。",
    author: "王五",
    publishTime: "2026-02-23",
    type: "weekly"
  }
]

// 过滤列表
const filteredList = computed(() => {
  return mockList.filter((item) => {
    // 按类型筛选
    if (selectedType.value !== "all" && item.type !== selectedType.value) {
      return false
    }
    // 按搜索词筛选
    if (searchQuery.value) {
      const query = searchQuery.value.toLowerCase()
      const matchTitle = item.title.toLowerCase().includes(query)
      const matchSummary = item.summary.toLowerCase().includes(query)
      if (!matchTitle && !matchSummary) return false
    }
    // 按日期段筛选
    if (dateRange.value && dateRange.value.length === 2) {
      const start = dateRange.value[0]
      const end = dateRange.value[1]
      // 简单按字符串比较日期
      if (item.publishTime < start || item.publishTime > end) {
        return false
      }
    }
    return true
  })
})
</script>

<template>
  <div class="h-full w-full flex flex-col p-24px bg-white">
    <!-- 顶部操作区 -->
    <div class="flex items-center gap-16px mb-20px shrink-0">
      <el-input
        v-model="searchQuery"
        placeholder="请输入搜索标题或内容"
        class="!w-[300px]"
        clearable
        :prefix-icon="Search"
      />
      <el-select v-model="selectedType" class="!w-[150px]">
        <el-option label="全部" value="all" />
        <el-option label="日报" value="daily" />
        <el-option label="周报" value="weekly" />
      </el-select>
      <div class="w-[240px]">
        <el-date-picker
          v-model="dateRange"
          type="daterange"
          range-separator="-"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          value-format="YYYY-MM-DD"
          style="width: 100%"
        />
      </div>
    </div>

    <!-- 列表区 -->
    <div class="flex-1 overflow-y-auto flex flex-col gap-16px pr-8px pb-16px">
      <div
        v-for="item in filteredList"
        :key="item.id"
        class="border border-solid border-gray-200  rounded-4px p-20px flex flex-col gap-16px bg-white shadow-sm"
      >
        <!-- 标题 -->
        <div class="text-16px font-bold text-gray-900 leading-tight">
          {{ item.title }}
        </div>

        <!-- 摘要内容 -->
        <div class="text-13px text-gray-500 leading-relaxed">
          {{ item.summary }}
        </div>

        <!-- 底部信息和操作 -->
        <div class="flex items-center justify-between mt-4px">
          <div class="flex items-center gap-24px text-12px text-gray-400">
            <div class="flex items-center gap-6px">
              <el-icon class="text-14px">
                <User />
              </el-icon>
              {{ item.author }}
            </div>
            <div>发布时间: {{ item.publishTime }}</div>
          </div>
        </div>
      </div>

      <!-- 无数据空状态 -->
      <el-empty v-if="filteredList.length === 0" description="暂无归档数据" />
    </div>
  </div>
</template>

<style scoped lang="scss"></style>
