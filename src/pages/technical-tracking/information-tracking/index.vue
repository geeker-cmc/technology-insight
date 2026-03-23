<script setup lang="ts">
import { BottomRight, DataLine, Document, Search, Star, StarFilled, TopRight } from "@element-plus/icons-vue"
import { ref } from "vue"
import RankingSidebar from "./components/RankingSidebar.vue"
import TrendChart from "./components/TrendChart.vue"

const searchQuery = ref("")

function handleSearch() {
  // TODO: 这里可以替换为实际的搜索过滤逻辑
  console.log("执行搜索，关键字: ", searchQuery.value)
}

const categoryList = [
  {
    title: "已关注技术领域",
    children: [
      { name: "天基太阳能", count: 0, active: true },
      { name: "数字电网", count: 0 },
      { name: "氢能应用", count: 0 },
      { name: "交通电气化", count: 0 },
      { name: "新能源发电", count: 0 }
    ]
  }
]

const articleList = [
  {
    category: "天基太阳能",
    xAxisData: ["2/18", "2/21", "2/24", "2/27", "3/2", "3/5", "3/8", "3/11", "3/14", "3/19"],
    trendData: [8000, 7500, 4000, 2000, 1500, 3000, 2000, 2500, 52000, 4000, 2000],
    newsList: [
      { title: "马斯克团队秘密调研中国光伏企业，HJT与钙钛矿技术成重点", date: "03-26" },
      { title: "马斯克团队秘密调研中国光伏企业，HJT与钙钛矿技术成重点", date: "03-26" },
      { title: "马斯克团队秘密调研中国光伏企业，HJT与钙钛矿技术成重点", date: "03-26" }
    ],
    index: "涉电耦合指数",
    stars: 5
  },
  {
    category: "可控核聚变",
    xAxisData: ["2/18", "2/21", "2/24", "2/27", "3/2", "3/5", "3/8", "3/11", "3/14", "3/19"],
    trendData: [3000, 3500, 4000, 5000, 15000, 45000, 20000, 10000, 8000, 6000],
    newsList: [
      { title: "中国\"人造太阳\"EAST实现403秒稳态等离子体运行，创世界纪录", date: "11-20" },
      { title: "全球最大ITER托卡马克反应堆核心部件成功完成安装", date: "10-15" },
      { title: "美国国家点火装置再次实现净能量增益突破", date: "09-08" }
    ],
    index: "涉电耦合指数",
    stars: 4
  },
  {
    category: "固态电池",
    xAxisData: ["2/18", "2/21", "2/24", "2/27", "3/2", "3/5", "3/8", "3/11", "3/14", "3/19"],
    trendData: [10000, 12000, 25000, 18000, 15000, 14000, 12000, 15000, 30000, 28000],
    newsList: [
      { title: "QuantumScape固态电池通过车企测试，循环寿命超1000次", date: "03-15" },
      { title: "宁德时代宣布新一代全固态电池能量密度再创新高", date: "03-10" },
      { title: "丰田计划2027年推出搭载固态电池的新型纯电动车", date: "02-28" }
    ],
    index: "涉电耦合指数",
    stars: 3
  }
]
</script>

<template>
  <div class="app-container flex h-full w-full overflow-hidden bg-[var(--v3-body-bg-color)] p-16px gap-16px">
    <!-- 左侧栏 -->
    <aside class="w-240px flex-shrink-0 bg-white rounded-8px overflow-y-auto shadow-sm">
      <div class="p-24px">
        <!-- 搜索框 -->
        <el-input
          v-model="searchQuery"
          placeholder="搜索指定领域"
          class="mb-24px"
          clearable
          @keyup.enter="handleSearch"
        >
          <template #suffix>
            <el-icon class="cursor-pointer" @click="handleSearch">
              <Search />
            </el-icon>
          </template>
        </el-input>

        <div v-for="(group, index) in categoryList" :key="index" class="mb-24px">
          <!-- 一级分类标题 -->
          <div class="flex items-center text-gray-400 text-13px mb-20px font-normal">
            <span>{{ group.title }}</span>
          </div>
          <!-- 二级分类列表 -->
          <div class="space-y-20px ml-4px">
            <div
              v-for="item in group.children"
              :key="item.name"
              class="flex items-center px-4px py-4px cursor-pointer text-gray-700"
            >
              <span class="text-15px">{{ item.name }}</span>
            </div>
          </div>
        </div>
      </div>
    </aside>

    <!-- 中间自适应栏 -->
    <main class="flex-1 min-w-0 overflow-hidden">
      <!-- 居中自适应容器：通过 min-w 和 mx-auto 实现 -->
      <div class="h-full min-w-800px max-w-1200px mx-auto flex flex-col">
        <!-- 内部卡片滚动区 -->
        <div class="flex-1 overflow-y-auto pr-8px">
          <div class="grid grid-cols-2 gap-16px p-4px">
            <div
              v-for="(article, index) in articleList"
              :key="index"
              class="bg-white rounded-12px p-20px shadow-sm flex flex-col border border-solid border-gray-100"
            >
              <!-- 顶部标签与收藏 -->
              <div class="flex items-center justify-between mb-16px">
                <span class="bg-[#EAEBFC] text-[#555CB5] text-14px px-12px py-6px rounded-4px font-bold">
                  {{ article.category }}
                </span>
                <el-icon class="text-gray-600 cursor-pointer" :size="24" :class="{ 'text-yellow-500': article.stars === 5 }">
                  <StarFilled v-if="article.stars === 5" />
                  <Star v-else />
                </el-icon>
              </div>

              <!-- 中间内容区（左右并列图表与文本） -->
              <div class="grid grid-cols-2 gap-16px mb-20px">
                <!-- 左侧：图表 -->
                <div class="flex flex-col min-w-0">
                  <div class="text-14px font-bold text-gray-800 mb-8px truncate">
                    近30天热点指数趋势
                  </div>
                  <div class="h-140px w-full">
                    <TrendChart :x-axis-data="article.xAxisData" :trend-data="article.trendData" />
                  </div>
                </div>

                <!-- 右侧：资讯热点 -->
                <div class="flex flex-col min-w-0">
                  <div class="text-14px font-bold text-gray-800 mb-8px truncate">
                    资讯热点
                  </div>
                  <div class="flex flex-col gap-8px justify-center flex-1">
                    <div
                      v-for="(news, idx) in article.newsList"
                      :key="idx"
                      class="flex justify-between items-start gap-8px cursor-pointer"
                    >
                      <span class="text-12px text-gray-700 leading-snug flex-1 line-clamp-3">{{ news.title }}</span>
                      <span class="text-12px text-gray-500 whitespace-nowrap pt-2px">{{ news.date }}</span>
                    </div>
                  </div>
                </div>
              </div>

              <!-- 底部指数面板 -->
              <div class="bg-[#FFF9E6] rounded-8px px-16px py-12px flex items-center justify-between mt-12px">
                <div class="flex items-center gap-8px text-[#E68A00] font-bold text-14px">
                  <span class="text-16px">⚡</span>
                  <span>{{ article.index }}</span>
                </div>
                <div class="flex gap-2px">
                  <span
                    v-for="i in 5"
                    :key="i"
                    class="text-18px"
                    :class="i <= article.stars ? 'text-yellow-400' : 'text-gray-800'"
                  >
                    ★
                  </span>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- 底部页码区 - 与卡片同属一个居中容器 -->
        <div class="h-60px flex-shrink-0 flex items-center justify-center bg-white mt-16px rounded-8px shadow-sm">
          <el-pagination background layout="prev, pager, next" :total="100" />
        </div>
      </div>
    </main>

    <!-- 右侧栏（已封装为组件） -->
    <RankingSidebar />
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
