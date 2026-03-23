<script setup lang="ts">
import { BottomRight, DataLine, Document, Search, Star, StarFilled, TopRight } from "@element-plus/icons-vue"
import { ref } from "vue"
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

const hotTopics = [
  { rank: 1, title: "太空光伏发电", hot: "9,876", index: "5.0", trend: "+23%", isUp: true },
  { rank: 2, title: "钠离子电池", hot: "8,543", index: "4.0", trend: "+18%", isUp: true },
  { rank: 3, title: "AI调度", hot: "7,321", index: "5.0", trend: "+31%", isUp: true },
  { rank: 4, title: "固态电池", hot: "6,234", index: "3.0", trend: "-5%", isUp: false },
  { rank: 5, title: "钙钛矿电池", hot: "5,432", index: "2.8", trend: "+12%", isUp: true },
  { rank: 6, title: "液流电池", hot: "4,567", index: "2.5", trend: "+8%", isUp: true },
  { rank: 7, title: "碳捕集与封存", hot: "3,890", index: "2.1", trend: "-2%", isUp: false },
  { rank: 8, title: "虚拟电厂", hot: "3,456", index: "1.9", trend: "+15%", isUp: true },
  { rank: 9, title: "特高压输电", hot: "2,987", index: "1.5", trend: "+5%", isUp: true },
  { rank: 10, title: "车网互动(V2G)", hot: "2,345", index: "1.2", trend: "-1%", isUp: false }
]

const hotNews = [
  { rank: 1, title: "太空光伏发电", desc: "江苏虚拟电厂参与电力市场交易机制创新试点获重大突破，江苏虚拟电厂参与电力市场交易机制创新试点获重大突破...", source: "国家电网报", date: "03-23", hot: "9,876", trend: "+23%", isUp: true },
  { rank: 2, title: "钠离子电池", desc: "最新研发的钠离子电池能量密度再创新高，预计将在未来的清洁能源储能中发挥重要作用，引发行业关注...", source: "人民网", date: "03-22", hot: "8,543", trend: "+18%", isUp: true },
  { rank: 3, title: "AI调度", desc: "利用人工智能算法对全网电力进行智能调度，极大地提升了电网运行的稳定性及效率，相关技术研究加速...", source: "新华社", date: "03-21", hot: "7,321", trend: "+31%", isUp: true },
  { rank: 4, title: "固态电池", desc: "某行业龙头企业宣布全固态电池产线已进入试生产阶段，此举将极大推动新能源汽车行业续航标准的提升...", source: "央视新闻", date: "03-20", hot: "6,234", trend: "-5%", isUp: false },
  { rank: 5, title: "钙钛矿电池", desc: "新一代钙钛矿光伏转换转化率突破30%，其极低的制造成本展现出了对传统硅基太阳能电池强大的替代潜力...", source: "科技日报", date: "03-19", hot: "5,432", trend: "+12%", isUp: true },
  { rank: 6, title: "液流电池", desc: "全钒液流电池百兆瓦级储能电站正式并网运行，长时储能应用场景下其安全性与寿命优势再次得到实际验证...", source: "人民日报", date: "03-18", hot: "4,567", trend: "+8%", isUp: true },
  { rank: 7, title: "碳捕集与封存", desc: "百万吨级CCUS（碳捕集、利用与封存）示范项目顺利落地，为传统高耗能行业绿色低碳转型提供了宝贵经验...", source: "中国能源报", date: "03-17", hot: "3,890", trend: "-2%", isUp: false },
  { rank: 8, title: "虚拟电厂", desc: "国家层面出台多项政策支持虚拟电厂项目建设，鼓励通过数字化手段提升局部地区电网对可再生能源的消纳能力...", source: "经济日报", date: "03-16", hot: "3,456", trend: "+15%", isUp: true },
  { rank: 9, title: "特高压输电", desc: "国内首条千万千瓦级特高压直流输电工程全线贯通，标志着跨区域电力大规模、长距离输送配置能力迈上新台阶...", source: "新华网", date: "03-15", hot: "2,987", trend: "+5%", isUp: true },
  { rank: 10, title: "车网互动(V2G)", desc: "某大型新能源车企与本地电网公司联合发布V2G商用试点，允许车主在电网用电高负荷时通过向电网反向放电来获取收益...", source: "中国证券报", date: "03-14", hot: "2,345", trend: "-1%", isUp: false }
]

const hotPolicies = [
  { rank: 1, title: "太空光伏发电", desc: "江苏虚拟电厂参与电力市场交易机制创新试点获重大突破，江苏虚拟电厂参与电力市场交易机制创新试点获重大突破...", source: "国务院", date: "03-23", hot: "9,876" },
  { rank: 2, title: "新型储能建设", desc: "国家发改委、国家能源局联合印发关于加快推动新型储能发展的指导意见，进一步明确了储能技术在新型电力系统中的核心地位...", source: "国家发改委", date: "03-22", hot: "8,543" },
  { rank: 3, title: "碳达峰碳中和", desc: "生态环境部发布关于促进应对气候变化投融资的指导意见，强调双碳目标下的技术创新支持力度...", source: "生态环境部", date: "03-21", hot: "7,321" },
  { rank: 4, title: "新能源汽车下乡", desc: "多部门联合启动新一轮新能源汽车下乡活动，配套充电基础设施建设成为本次政策支持的重中之重...", source: "工信部", date: "03-20", hot: "6,234" },
  { rank: 5, title: "微电网建设指南", desc: "地方政府联合发布微电网建设规划，计划在未来三年内打造百个高质量微电网示范园区...", source: "发改委", date: "03-19", hot: "5,432" },
  { rank: 6, title: "绿电绿证交易", desc: "关于进一步做好绿色电力交易与绿证落实通知发布，要求扩大绿电参与市场交易的规模...", source: "国家能源局", date: "03-18", hot: "4,567" },
  { rank: 7, title: "氢能产业发展示范", desc: "《氢能产业发展中长期规划（2021-2035年）》相关配套落地细则出台，加速氢能应用商业化...", source: "国家发改委", date: "03-17", hot: "3,890" },
  { rank: 8, title: "分布式光伏补贴", desc: "为进一步促进分布式光伏发展，部分地区出台了针对工商业及户用光伏的新一轮补贴政策...", source: "财政部", date: "03-16", hot: "3,456" },
  { rank: 9, title: "电力现货市场", desc: "相关部门出台完善电力现货市场基本规则的通知，为新能源主体全面入市提供了政策依据...", source: "国家能源局", date: "03-15", hot: "2,987" },
  { rank: 10, title: "特高压审批加速", desc: "为保障跨省区能源调配，相关部门明确指出年内将加速推进多条特高压线路的审批与核准流程...", source: "国家发改委", date: "03-14", hot: "2,345" }
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

    <!-- 右侧栏 -->
    <aside class="w-300px flex-shrink-0 bg-white rounded-8px overflow-y-auto shadow-sm">
      <div class="p-16px">
        <div class="flex items-center gap-8px mb-12px">
          <span class="text-18px font-bold">24小时热点技术</span>
        </div>

        <div class="space-y-6px">
          <div
            v-for="item in hotTopics"
            :key="item.rank"
            class="p-16px border border-[#E0E0E0] rounded-12px shadow-sm transition-shadow border-solid"
          >
            <div class="flex items-center gap-12px mb-8px">
              <div
                class="w-24px h-24px rounded-full flex items-center justify-center text-12px font-bold"
                :class="[
                  item.rank === 1 ? 'bg-[#FFC107] text-white'
                  : item.rank === 2 ? 'bg-[#C0C0C0] text-white'
                    : item.rank === 3 ? 'bg-[#CD7F32] text-white'
                      : 'bg-gray-200 text-gray-500',
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

        <!-- 24小时热点资讯 -->
        <div class="flex items-center gap-8px mb-12px mt-24px">
          <span class="text-18px font-bold">24小时热点资讯</span>
        </div>

        <div class="space-y-12px">
          <div
            v-for="item in hotNews"
            :key="item.rank"
            class="p-16px border border-[#E0E0E0] rounded-12px shadow-sm bg-white border-solid"
          >
            <!-- 头部 -->
            <div class="flex items-center gap-12px mb-12px">
              <div
                class="w-24px h-24px rounded-full flex items-center justify-center text-12px font-bold flex-shrink-0"
                :class="[
                  item.rank === 1 ? 'bg-[#FFC107] text-white'
                  : item.rank === 2 ? 'bg-[#C0C0C0] text-white'
                    : item.rank === 3 ? 'bg-[#CD7F32] text-white'
                      : 'bg-gray-200 text-gray-500',
                ]"
              >
                {{ item.rank }}
              </div>
              <div class="text-16px font-bold text-gray-800 line-clamp-1">
                {{ item.title }}
              </div>
            </div>

            <!-- 正文摘要 -->
            <div class="text-13px text-gray-500 leading-relaxed mb-16px line-clamp-2">
              {{ item.desc }}
            </div>

            <!-- 底部数据信息 -->
            <div class="flex items-center justify-between text-12px text-gray-400">
              <div class="flex items-center gap-4px">
                <span>{{ item.source }} · {{ item.date }}</span>
              </div>

              <div class="flex items-center gap-12px">
                <div class="flex items-center gap-4px">
                  <span>热度: {{ item.hot }}</span>
                </div>
                <div :class="item.isUp ? 'text-green-500' : 'text-red-500'" class="flex items-center font-medium">
                  <el-icon v-if="item.isUp" class="mr-2px">
                    <TopRight />
                  </el-icon>
                  <el-icon v-else class="mr-2px">
                    <BottomRight />
                  </el-icon>
                  <span>{{ item.trend }}</span>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- 24小时热点政策 -->
        <div class="flex items-center gap-8px mb-12px mt-24px">
          <span class="text-18px font-bold">24小时热点政策</span>
        </div>

        <div class="space-y-12px pb-16px">
          <div
            v-for="item in hotPolicies"
            :key="item.rank"
            class="p-16px border border-solid border-[#E0E0E0] rounded-12px shadow-sm bg-white"
          >
            <!-- 头部 -->
            <div class="flex items-center gap-12px mb-12px">
              <div
                class="w-24px h-24px rounded-full flex items-center justify-center text-12px font-bold flex-shrink-0"
                :class="[
                  item.rank === 1 ? 'bg-[#FFC107] text-white'
                  : item.rank === 2 ? 'bg-[#C0C0C0] text-white'
                    : item.rank === 3 ? 'bg-[#CD7F32] text-white'
                      : 'bg-gray-200 text-gray-500',
                ]"
              >
                {{ item.rank }}
              </div>
              <div class="text-16px font-bold text-gray-800 line-clamp-1">
                {{ item.title }}
              </div>
            </div>

            <!-- 正文摘要 -->
            <div class="text-13px text-gray-500 leading-relaxed mb-16px line-clamp-2">
              {{ item.desc }}
            </div>

            <!-- 底部数据信息 -->
            <div class="flex items-center justify-between text-12px text-gray-400">
              <div class="flex items-center gap-4px">
                <el-icon class="text-14px">
                  <Document />
                </el-icon>
                <span>{{ item.source }} · {{ item.date }}</span>
              </div>

              <div class="flex items-center gap-4px">
                <el-icon class="text-14px">
                  <DataLine />
                </el-icon>
                <span>热度: {{ item.hot }}</span>
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
