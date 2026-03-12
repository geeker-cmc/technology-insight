<script setup lang="ts">
const categoryList = [
  {
    title: "技术领域",
    children: [
      { name: "新型储能", count: 128, active: true },
      { name: "数字电网", count: 95 },
      { name: "氢能应用", count: 76 },
      { name: "交通电气化", count: 112 },
      { name: "新能源发电", count: 143 }
    ]
  },
  {
    title: "新兴储能技术",
    children: [
      { name: "钠离子电池", count: 23 },
      { name: "固态电池", count: 45 },
      { name: "液流电池", count: 18 }
    ]
  },
  {
    title: "数字电网技术",
    children: [
      { name: "虚拟电厂", count: 34 },
      { name: "AI调度", count: 28 },
      { name: "边缘计算", count: 21 }
    ]
  }
]

const hotTopics = [
  { rank: 1, title: "太空光伏发电", hot: "9,876", index: "5.0", trend: "+23%", isUp: true },
  { rank: 2, title: "钠离子电池", hot: "8,543", index: "4.0", trend: "+18%", isUp: true },
  { rank: 3, title: "AI调度", hot: "7,321", index: "5.0", trend: "+31%", isUp: true },
  { rank: 4, title: "固态电池", hot: "6,234", index: "3.0", trend: "-5%", isUp: false }
]

const articleList = [
  {
    category: "太空光伏发电",
    title: "马斯克团队秘密调研中国光伏企业，HJT与钙钛矿技术成重点",
    description: "据知情人士透露，马斯克团队近期秘密考察了晶科能源、TCL中环、京盛机电等中国顶尖光伏企业，重点关注HJT（异质结）和钙钛矿技术的生产线...",
    tags: ["太空能源", "新能源发电", "HJT技术", "钙钛矿"],
    index: "涉电耦合指数",
    stars: 4,
    nodes: "主要耦合环节：发电侧太空能源/分布式光伏 | 支撑文献：287篇",
    source: "新浪财经",
    time: "2小时前",
    views: "18,456"
  },
  {
    category: "可控核聚变",
    title: "中国\"人造太阳\"EAST实现403秒稳态等离子体运行，创世界纪录",
    description: "中科院等离子体所宣布，全超导托卡马克核聚变实验装置EAST成功实现403秒稳态高约束模式等离子体运行，打破此前的101秒世界纪录...",
    tags: ["新型电力系统", "清洁能源发电", "等离子体约束"],
    index: "涉电耦合指数",
    stars: 5,
    nodes: "主要耦合环节：发电侧基荷电源 | 支撑文献：124篇",
    source: "中科院",
    time: "3天前",
    views: "12,341"
  },
  {
    category: "固态电池",
    title: "QuantumScape固态电池通过车企测试，循环寿命超1000次",
    description: "QuantumScape宣布其固态电池已通过多家欧洲车企的测试验证，在保持高能量密度的同时，循环寿命超过1000次，有望在2025年实现商业化量产...",
    tags: ["交通电气化", "新能源装备", "储能技术"],
    index: "涉电耦合指数",
    stars: 3,
    nodes: "主要耦合环节：用电侧电动汽车 | 支撑文献：38篇",
    source: "路透社",
    time: "2天前",
    views: "3,421"
  },
  {
    category: "AI调度",
    title: "DeepMind AI优化算法降低电网调度成本15%",
    description: "DeepMind与英国国家电网合作开发的AI调度系统在英国电网试点中取得成功，通过强化学习算法优化发电机组启停和负荷分配，降低运营成本15%...",
    tags: ["新型电力系统", "工业源管理", "深度学习", "强化学习"],
    index: "涉电耦合指数",
    stars: 5,
    nodes: "主要耦合环节：全网调度优化 | 支撑文献：67篇",
    source: "MIT Technology Review",
    time: "3天前",
    views: "4,392"
  },
  {
    category: "固态电池",
    title: "QuantumScape固态电池通过车企测试，循环寿命超1000次",
    description: "QuantumScape宣布其固态电池已通过多家欧洲车企的测试验证，在保持高能量密度的同时，循环寿命超过1000次，有望在2025年实现商业化量产...",
    tags: ["交通电气化", "新能源装备", "储能技术"],
    index: "涉电耦合指数",
    stars: 3,
    nodes: "主要耦合环节：用电侧电动汽车 | 支撑文献：38篇",
    source: "路透社",
    time: "2天前",
    views: "3,421"
  }

]
</script>

<template>
  <div class="app-container flex h-full w-full overflow-hidden bg-[var(--v3-body-bg-color)] p-16px gap-16px">
    <!-- 左侧栏 -->
    <aside class="w-300px flex-shrink-0 bg-white rounded-8px overflow-y-auto shadow-sm">
      <div class="p-16px">
        <div v-for="(group, index) in categoryList" :key="index" class="mb-24px">
          <!-- 一级分类标题 -->
          <div class="flex items-center text-gray-400 text-14px mb-12px font-bold">
            <span class="ml-4px">{{ group.title }}</span>
          </div>
          <!-- 二级分类列表 -->
          <div class="space-y-4px">
            <div
              v-for="item in group.children"
              :key="item.name"
              class="flex items-center justify-between px-12px py-10px rounded-6px cursor-pointer transition-colors" :class="[
                item.active ? 'bg-blue-50 text-blue-600' : 'text-gray-700',
              ]"
            >
              <div class="flex items-center">
                <span class="text-15px font-medium">{{ item.name }}</span>
              </div>
              <span
                class="px-8px py-2px rounded-full text-12px" :class="[
                  item.active ? 'bg-blue-200 text-blue-700' : 'bg-gray-100 text-gray-400',
                ]"
              >
                {{ item.count }}
              </span>
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
              class="bg-white rounded-12px p-20px shadow-sm flex flex-col gap-12px border border-gray-100"
            >
              <!-- 顶部标签 -->
              <div class="flex items-center">
                <span class="bg-blue-50 text-blue-500 text-12px px-8px py-4px rounded-4px font-medium">
                  {{ article.category }}
                </span>
              </div>
              <!-- 标题 -->
              <h3 class="text-18px font-bold text-gray-800 leading-snug line-clamp-2">
                {{ article.title }}
              </h3>
              <!-- 描述 -->
              <p class="text-14px text-gray-500 leading-relaxed line-clamp-3">
                {{ article.description }}
              </p>
              <!-- 中间标签组 -->
              <div class="flex flex-wrap gap-8px mt-4px">
                <span
                  v-for="tag in article.tags"
                  :key="tag"
                  class="bg-gray-50 text-gray-500 text-12px px-8px py-4px rounded-4px border border-gray-100"
                >
                  {{ tag }}
                </span>
              </div>
              <!-- 指数面板 -->
              <div class="bg-yellow-50 rounded-8px p-12px border border-yellow-100">
                <div class="flex items-center justify-between mb-8px">
                  <div class="flex items-center gap-4px text-orange-500 font-bold text-14px">
                    <span>⚡</span>
                    <span>{{ article.index }}</span>
                  </div>
                  <div class="flex gap-2px">
                    <span v-for="i in 5" :key="i" class="text-14px" :class="i <= article.stars ? 'text-yellow-400' : 'text-gray-200'">
                      ★
                    </span>
                  </div>
                </div>
                <p class="text-12px text-orange-400 opacity-80">
                  {{ article.nodes }}
                </p>
              </div>
              <!-- 底部信息 -->
              <div class="flex items-center justify-between mt-auto pt-10px border-t border-gray-50 text-12px text-gray-400">
                <div class="flex items-center gap-12px">
                  <span>{{ article.source }}</span>
                  <span>{{ article.time }}</span>
                </div>
                <span>{{ article.views }}</span>
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

    <!-- 右侧栏 -->
    <aside class="w-300px flex-shrink-0 bg-white rounded-8px overflow-y-auto shadow-sm">
      <div class="p-16px">
        <div class="flex items-center gap-8px mb-12px">
          <span class="text-18px font-bold">24小时热点技术主题榜</span>
        </div>
        <div class="text-gray-400 text-12px mb-16px uppercase tracking-wider">
          TOP 10 技术主题实时热度
        </div>

        <div class="space-y-16px">
          <div
            v-for="item in hotTopics"
            :key="item.rank"
            class="p-16px border border-[#E0E0E0] rounded-12px shadow-sm transition-shadow border-solid"
          >
            <div class="flex items-center gap-12px mb-8px">
              <div
                class="w-24px h-24px rounded-full flex items-center justify-center text-12px font-bold"
                :class="[
                  item.rank <= 3 ? 'bg-orange-500 text-white' : 'bg-gray-200 text-gray-500',
                ]"
              >
                {{ item.rank }}
              </div>
              <div class="text-16px font-bold text-gray-800">
                {{ item.title }}
              </div>
            </div>
            <div class="flex items-center gap-12px text-12px text-gray-500">
              <div class="flex items-center gap-4px">
                <span>热度: {{ item.hot }}</span>
              </div>
              <div>耦合指数 {{ item.index }}</div>
              <div :class="item.isUp ? 'text-green-500' : 'text-red-500'">
                {{ item.isUp ? '↑' : '↓' }} {{ item.trend }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </aside>
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
