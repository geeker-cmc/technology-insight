<script setup lang="ts">
import { DataLine, Document, HotWater, OfficeBuilding, Share, StarFilled, User, View, WarnTriangleFilled } from "@element-plus/icons-vue"

// 热门资讯
const hotNewsList = [
  { rank: 1, title: "江苏虚拟电厂参与电力市场交易机制创新试点获重大突破", source: "国家电网报", time: "2天前", views: "12.5K", shares: "256" },
  { rank: 2, title: "虚拟电厂助力浙江迎峰度夏 聚合负载响应能力超百万千瓦", source: "浙江日报", time: "3天前", views: "9.8K", shares: "189" },
  { rank: 3, title: "华为发布新一代虚拟电厂解决方案 融合AI实现智能调度", source: "科技日报", time: "4天前", views: "8.2K", shares: "145" },
  { rank: 4, title: "上海启动VPP2.0示范项目 车网互动技术首次大规模应用", source: "解放日报", time: "5天前", views: "7.1K", shares: "112" },
  { rank: 5, title: "清华大学团队提出虚拟电厂分布式优化算法 提升调度效率30%", source: "学术期刊", time: "6天前", views: "6.3K", shares: "98" },
  { rank: 6, title: "南方电网虚拟电厂平台接入分布式资源超5000个", source: "南方电网报", time: "1周前", views: "5.6K", shares: "87" },
  { rank: 1, title: "江苏虚拟电厂参与电力市场交易机制创新试点获重大突破", source: "国家电网报", time: "2天前", views: "12.5K", shares: "256" },
  { rank: 2, title: "虚拟电厂助力浙江迎峰度夏 聚合负载响应能力超百万千瓦", source: "浙江日报", time: "3天前", views: "9.8K", shares: "189" },
  { rank: 3, title: "华为发布新一代虚拟电厂解决方案 融合AI实现智能调度", source: "科技日报", time: "4天前", views: "8.2K", shares: "145" },
  { rank: 4, title: "上海启动VPP2.0示范项目 车网互动技术首次大规模应用", source: "解放日报", time: "5天前", views: "7.1K", shares: "112" },
  { rank: 5, title: "清华大学团队提出虚拟电厂分布式优化算法 提升调度效率30%", source: "学术期刊", time: "6天前", views: "6.3K", shares: "98" },
  { rank: 6, title: "南方电网虚拟电厂平台接入分布式资源超5000个", source: "南方电网报", time: "1周前", views: "5.6K", shares: "87" }
]

// 活跃机构
const orgList = [
  { char: "国", name: "国家电网公司", type: "中央企业 · 电力行业", articles: 23, rating: 4.8 },
  { char: "南", name: "南方电网", type: "中央企业 · 电力行业", articles: 18, rating: 4.5 },
  { char: "华", name: "华为数字能源", type: "民营企业 · 能源科技", articles: 12, rating: 4.2 },
  { char: "清", name: "清华大学电机系", type: "高等院校 · 电力系统", articles: 15, rating: 4.9 },
  { char: "中", name: "中国电科院", type: "科研院所 · 电力研究", articles: 11, rating: 4.6 }
]

// 活跃人物
const peopleList = [
  { char: "张", name: "张明", title: "清华大学电机系 · 教授", articles: 8, impact: 156 },
  { char: "李", name: "李华", title: "浙江大学电气学院 · 教授", articles: 6, impact: 98 },
  { char: "王", name: "王伟", title: "华北电力大学 · 副教授", articles: 5, impact: 67 },
  { char: "刘", name: "刘芳", title: "天津大学 · 博士后", articles: 4, impact: 45 }
]
</script>

<template>
  <div class="h-full grid grid-cols-3 gap-24px min-h-0">
    <!-- 1. 热门资讯 -->
    <div class="bg-white rounded-8px border border-solid border-gray-100 p-20px flex flex-col min-h-0">
      <!-- 标题 -->
      <div class="flex items-center gap-8px mb-20px text-16px font-bold text-gray-800 shrink-0">
        <el-icon class="text-orange-500 text-18px">
          <HotWater />
        </el-icon>
        热门资讯
      </div>
      <!-- 列表 -->
      <div class="space-y-16px flex-1 overflow-y-auto hide-scrollbar">
        <div
          v-for="item in hotNewsList"
          :key="item.rank"
          class="p-16px rounded-8px border border-solid border-gray-100 cursor-pointer bg-white shadow-sm flex flex-col gap-12px"
        >
          <!-- Top: rank + title -->
          <div class="flex items-start gap-12px">
            <span
              class="w-20px h-20px flex items-center justify-center rounded-4px text-12px font-bold mt-2px shrink-0"
              :class="item.rank <= 3 ? 'bg-[#FFF0E6] text-[#FF8C00]' : 'bg-gray-100 text-gray-500'"
            >
              {{ item.rank }}
            </span>
            <div class="text-14px font-bold text-gray-800 leading-snug line-clamp-2">
              {{ item.title }}
            </div>
          </div>
          <!-- Middle: source/time -->
          <div class="flex items-center gap-6px text-12px text-gray-500 mt-auto ml-32px">
            <el-icon><OfficeBuilding /></el-icon>
            <span>{{ item.source }} · {{ item.time }}</span>
          </div>
          <!-- Bottom: stats -->
          <div class="flex items-center gap-16px text-12px mt-2px ml-32px font-bold">
            <div class="flex items-center gap-4px text-red-500">
              <el-icon><View /></el-icon>
              <span>{{ item.views }}</span>
            </div>
            <div class="flex items-center gap-4px text-blue-500">
              <el-icon><Share /></el-icon>
              <span>{{ item.shares }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- 2. 活跃机构 -->
    <div class="bg-white rounded-8px border border-solid border-gray-100 p-20px flex flex-col min-h-0">
      <!-- 标题 -->
      <div class="flex items-center gap-8px mb-20px text-16px font-bold text-gray-800 shrink-0">
        <el-icon class="text-[#555CB5] text-18px">
          <OfficeBuilding />
        </el-icon>
        活跃机构
      </div>
      <!-- 列表 -->
      <div class="space-y-16px flex-1 overflow-y-auto hide-scrollbar">
        <div
          v-for="(item, index) in orgList"
          :key="index"
          class="p-16px rounded-8px border border-solid border-gray-100 cursor-pointer bg-white shadow-sm flex items-center gap-16px"
        >
          <!-- Logo 方块 -->
          <div class="w-48px h-48px shrink-0 rounded-8px bg-[#EAEBFC] text-[#555CB5] text-20px font-bold flex items-center justify-center">
            {{ item.char }}
          </div>
          <!-- 右侧内容 -->
          <div class="flex-1 min-w-0 flex flex-col gap-6px">
            <div class="text-15px font-bold text-gray-800 truncate">
              {{ item.name }}
            </div>
            <div class="text-12px text-gray-500 truncate mt-2px">
              {{ item.type }}
            </div>
            <div class="flex items-center gap-16px text-12px text-gray-500 font-bold mt-4px">
              <div class="flex items-center gap-4px">
                <el-icon><Document /></el-icon>
                <span>{{ item.articles }} 篇</span>
              </div>
              <div class="flex items-center gap-4px">
                <el-icon class="text-gray-400">
                  <StarFilled />
                </el-icon>
                <span>{{ item.rating }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- 3. 活跃人物 -->
    <div class="bg-white rounded-8px border border-solid border-gray-100 p-20px flex flex-col min-h-0">
      <!-- 标题 -->
      <div class="flex items-center gap-8px mb-20px text-16px font-bold text-gray-800 shrink-0">
        <el-icon class="text-[#18A058] text-18px">
          <User />
        </el-icon>
        活跃人物
      </div>
      <!-- 列表 -->
      <div class="space-y-16px flex-1 overflow-y-auto hide-scrollbar">
        <div
          v-for="(item, index) in peopleList"
          :key="index"
          class="p-16px rounded-8px border border-solid border-gray-100 cursor-pointer bg-white shadow-sm flex items-center gap-16px"
        >
          <!-- 圆形头像 -->
          <div class="w-48px h-48px shrink-0 rounded-full bg-[#E8F8EE] text-[#18A058] text-20px font-bold flex items-center justify-center">
            {{ item.char }}
          </div>
          <!-- 右侧内容 -->
          <div class="flex-1 min-w-0 flex flex-col gap-6px">
            <div class="text-15px font-bold text-gray-800 truncate">
              {{ item.name }}
            </div>
            <div class="text-12px text-gray-500 truncate mt-2px">
              {{ item.title }}
            </div>
            <div class="flex items-center gap-16px text-12px text-gray-500 font-bold mt-4px">
              <div class="flex items-center gap-4px">
                <el-icon><Document /></el-icon>
                <span>{{ item.articles }} 篇</span>
              </div>
              <div class="flex items-center gap-4px">
                <el-icon class="text-gray-400">
                  <DataLine />
                </el-icon>
                <span>{{ item.impact }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.hide-scrollbar {
  -ms-overflow-style: none; /* IE and Edge */
  scrollbar-width: none; /* Firefox */
}
.hide-scrollbar::-webkit-scrollbar {
  display: none; /* Chrome, Safari and Opera */
}
</style>
