<script setup lang="ts">
import { Back, InfoFilled } from "@element-plus/icons-vue"
import { useRoute, useRouter } from "vue-router"
import { useTagsViewStore } from "@/pinia/stores/tags-view"
import TrendChart from "../../information-tracking/components/TrendChart.vue"

const route = useRoute()
const router = useRouter()
const tagsViewStore = useTagsViewStore()

function goBack() {
  // 关闭当前标签页缓存与记录
  tagsViewStore.delVisitedView(route)
  tagsViewStore.delCachedView(route)
  // 执行返回
  router.back()
}

// 模拟图表数据
const xAxisData = ["2/18", "2/21", "2/24", "2/27", "3/2", "3/5", "3/8", "3/11", "3/14", "3/19"]
const trendData = [5000, 3000, 1000, 800, 4000, 1500, 1500, 55000, 4000, 1500]
</script>

<template>
  <div class="bg-white rounded-8px shadow-sm overflow-hidden border border-solid border-gray-100">
    <!-- Header -->
    <div class="px-24px py-20px flex items-center justify-between">
      <div class="flex items-start gap-24px">
        <!-- <div class="flex items-center gap-4px text-gray-500 cursor-pointer pt-6px hover:text-blue-500 transition-colors" @click="goBack">
          <el-icon><Back /></el-icon>
          <span class="text-14px">返回</span>
        </div> -->
        <div>
          <div class="text-24px font-bold text-gray-800 mb-6px">
            虚拟电厂
          </div>
          <div class="text-12px text-gray-400">
            Virtual Power Plant (VPP) · 技术主题深度分析
          </div>
        </div>
      </div>

      <div>
        <div class="bg-[#EAEBFC] text-[#555CB5] px-16px py-8px rounded-6px flex items-center gap-8px text-14px font-bold">
          <span class="text-16px text-yellow-500">⚡</span> 涉电耦合指数 5.0
        </div>
      </div>
    </div>

    <!-- 蓝色分隔线 -->
    <div class="h-2px bg-[#3b82f6] w-full" />

    <!-- Content -->
    <div class="px-24px py-24px flex gap-48px">
      <!-- Left: Intro text -->
      <div class="flex-1">
        <div class="flex items-center gap-8px mb-16px text-16px font-bold text-gray-800">
          <el-icon class="text-blue-500 text-18px">
            <InfoFilled />
          </el-icon>
          技术介绍
        </div>
        <div class="text-14px text-gray-600 leading-8 space-y-16px text-justify">
          <p>虚拟电厂（Virtual Power Plant, VPP）是一种通过先进的信息通信技术和软件系统，将分布式电源、储能系统、可控负荷等分布式能源资源聚合起来，作为一个特殊电厂参与电网运行和电力市场协调管理的智慧能源系统。</p>
          <p>VPP通过AI优化算法实现能源的智能调度，既能提升可再生能源消纳能力，又能为电网提供调频、调峰等辅助服务，是构建新型电力系统的关键技术之一。它打破了传统发电厂集中式的生产模式，实现了能源生产的去中心化和智能化管理。</p>
          <p>当前虚拟电厂技术已在江苏、浙江、上海、广东等多个省份开展试点示范，聚合负荷规模从数万千瓦到数十万千瓦不等。技术路径已基本验证，涵盖工业需求响应、分布式新能源聚合、储能优化调度、电动汽车充电网络等多个应用场景，正在向规模化应用推进。</p>
        </div>
      </div>

      <!-- Right: Chart placeholder -->
      <div class="w-400px shrink-0 flex flex-col border-l border-solid border-gray-100 pl-48px">
        <div class="flex-1 w-full min-h-200px">
          <TrendChart :x-axis-data="xAxisData" :trend-data="trendData" />
        </div>
      </div>
    </div>
  </div>
</template>
