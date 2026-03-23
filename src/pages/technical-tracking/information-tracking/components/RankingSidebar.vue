<script setup lang="ts">
import { BottomRight, DataLine, Document, TopRight } from "@element-plus/icons-vue"

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
</script>

<template>
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
              <el-icon class="text-14px">
                <Document />
              </el-icon>
              <span>{{ item.source }} · {{ item.date }}</span>
            </div>

            <div class="flex items-center gap-12px">
              <div class="flex items-center gap-4px">
                <el-icon class="text-14px">
                  <DataLine />
                </el-icon>
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
</template>

<style scoped lang="scss">
.overflow-y-auto {
  -ms-overflow-style: none; /* IE and Edge */
  scrollbar-width: none; /* Firefox */

  &::-webkit-scrollbar {
    display: none; /* Chrome, Safari and Opera */
  }
}
</style>
