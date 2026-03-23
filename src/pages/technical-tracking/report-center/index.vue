<script setup lang="ts">
import { ref } from "vue"
import { useRouter } from "vue-router"

const router = useRouter()

const reportCategories = [
  {
    title: "报告类型",
    children: [
      { name: "每日科技动态", active: true },
      { name: "战略信息周报" }
    ]
  },
  {
    title: "生产状态",
    children: [
      { name: "草稿箱" },
      { name: "已归档" }
    ]
  }
]

const dateRange = ref([])
const infoList = [
  { title: "全球能源互联网发展趋势报告", time: "2024-03-20" },
  { title: "新型电力系统关键技术研究", time: "2024-03-19" },
  { title: "储能电池产业白皮书", time: "2024-03-18" },
  { title: "氢能交通应用示范项目分析", time: "2024-03-17" },
  { title: "数字孪生电网演进路线图", time: "2024-03-16" }
]

const reportList = [
  {
    tag: "大模型",
    tagColor: "blue",
    title: "DeepSeek-R2 技术报告披露：推理效率提升 340%，成本降至 GPT-4 的 1/20",
    description: "DeepSeek 在 arXiv 发布 R2 技术报告，采用全新 MoE 架构与推理链压缩技术，基准测试数学与代码能力超越 GPT-4o，同时将推理成本压降至行业最低水平，引发国际学界广泛关注。",
    source: "arXiv",
    time: "09:42"
  },
  {
    tag: "固态电池",
    tagColor: "green",
    title: "宁德时代确认 2026 年量产全固态电池，能量密度突破 500Wh/kg",
    description: "宁德时代发布公告确认全固态电池 2026 年 Q2 小批量量产，能量密度 500Wh/kg 较当前锂离子电池翻番，将首先供货高端乘用车与低空飞行器。",
    source: "公司公告",
    time: "11:15"
  },
  {
    tag: "量子计算",
    tagColor: "purple",
    title: "中科院发布首个实用化容错量子处理器，72 量子比特错误率低于 0.1%",
    description: "中科院团队在《Science》发布研究成果，72 量子比特处理器在实用化任务中错误率首次低于 0.1% 阈值，标志容错量子计算进入工程实现阶段。",
    source: "Science",
    time: "08:30"
  },
  {
    tag: "大模型",
    tagColor: "blue",
    title: "DeepSeek-R2 技术报告披露：推理效率提升 340%，成本降至 GPT-4 的 1/20",
    description: "DeepSeek 在 arXiv 发布 R2 技术报告，采用全新 MoE 架构与推理链压缩技术，基准测试数学与代码能力超越 GPT-4o，同时将推理成本压降至行业最低水平，引发国际学界广泛关注。",
    source: "arXiv",
    time: "09:42"
  },
  {
    tag: "固态电池",
    tagColor: "green",
    title: "宁德时代确认 2026 年量产全固态电池，能量密度突破 500Wh/kg",
    description: "宁德时代发布公告确认全固态电池 2026 年 Q2 小批量量产，能量密度 500Wh/kg 较当前锂离子电池翻番，将首先供货高端乘用车与低空飞行器。",
    source: "公司公告",
    time: "11:15"
  },
  {
    tag: "量子计算",
    tagColor: "purple",
    title: "中科院发布首个实用化容错量子处理器，72 量子比特错误率低于 0.1%",
    description: "中科院团队在《Science》发布研究成果，72 量子比特处理器在实用化任务中错误率首次低于 0.1% 阈值，标志容错量子计算进入工程实现阶段。",
    source: "Science",
    time: "08:30"
  }
]

/** 跳转至编辑日报页面 */
function handleGenerateReport() {
  router.push("/technical-tracking/edit-daily-report")
}
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
              :key="item.name"
              class="relative flex items-center justify-between px-16px py-12px rounded-8px cursor-pointer transition-colors"
              :class="[
                item.active ? 'bg-[#E3F2FD] text-[#1976D2]' : 'text-[#333333]',
              ]"
            >
              <!-- 激活态左侧指示条 -->
              <div v-if="item.active" class="absolute left-0 top-10px bottom-10px w-4px bg-[#1976D2] rounded-r-4px" />

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

    <!-- 中间自适应栏 -->
    <main class="flex-1 min-w-0 overflow-hidden">
      <!-- 居中自适应容器：通过 min-w 和 mx-auto 实现 -->
      <div class="h-full min-w-800px max-w-1200px mx-auto flex flex-col bg-white rounded-12px shadow-sm border border-gray-100 overflow-hidden border-solid">
        <!-- 头部信息统计 -->
        <div class="px-24px py-20px border-b border-gray-50 flex items-center gap-40px">
          <div class="flex items-center gap-8px">
            <span class="text-gray-400 text-14px">今日收录条目</span>
            <span class="text-24px font-bold text-blue-600">128</span>
          </div>
          <div class="flex items-center gap-8px">
            <span class="text-gray-400 text-14px">覆盖技术领域</span>
            <span class="text-24px font-bold text-gray-800">12</span>
          </div>
        </div>

        <!-- 内部内容滚动区 -->
        <div class="flex-1 overflow-y-auto p-24px">
          <div class="space-y-32px">
            <div v-for="(item, index) in reportList" :key="index" class="flex gap-20px">
              <!-- 复选框占位 -->
              <div class="pt-24px">
                <el-checkbox size="large" />
              </div>
              <div class="flex-1 space-y-10px">
                <!-- 标签 -->
                <div>
                  <span
                    class="text-12px px-8px py-3px rounded-4px font-medium"
                    :class="{
                      'bg-blue-50 text-blue-600': item.tagColor === 'blue',
                      'bg-green-50 text-green-600': item.tagColor === 'green',
                      'bg-purple-50 text-purple-600': item.tagColor === 'purple',
                    }"
                  >
                    {{ item.tag }}
                  </span>
                </div>
                <!-- 标题 -->
                <h3 class="text-18px font-bold text-gray-800 leading-snug">
                  {{ item.title }}
                </h3>
                <!-- 描述 -->
                <p class="text-14px text-gray-500 leading-relaxed">
                  {{ item.description }}
                </p>
                <!-- 底部来源与时间 -->
                <div class="flex items-center gap-24px text-12px text-gray-400">
                  <span class="flex items-center gap-4px">{{ item.source }}</span>
                  <span class="flex items-center gap-4px">{{ item.time }}</span>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- 底部页码区 -->
        <div class="h-70px flex-shrink-0 flex items-center justify-center border-t border-gray-50">
          <el-pagination background layout="prev, pager, next" :total="100" />
        </div>
      </div>
    </main>

    <!-- 右侧栏 -->
    <aside class="w-300px flex-shrink-0 flex flex-col gap-16px overflow-hidden">
      <!-- 上部分：日报操作卡片 -->
      <div class="bg-white rounded-8px p-16px shadow-sm border border-gray-100 flex-shrink-0">
        <div class="text-16px font-bold mb-16px text-gray-800">
          日报操作
        </div>
        <div class="space-y-16px">
          <div>
            <div class="text-12px text-gray-400 mb-8px">
              选择日期范围
            </div>
            <el-date-picker
              v-model="dateRange"
              type="daterange"
              range-separator="至"
              start-placeholder="开始日期"
              end-placeholder="结束日期"
              size="default"
              class="!w-full"
            />
          </div>
          <div class="flex gap-10px">
            <el-button type="primary" class="flex-1">
              查询信息
            </el-button>
            <el-button type="success" class="flex-1" @click="handleGenerateReport">
              生成日报
            </el-button>
          </div>
        </div>
      </div>

      <!-- 下部分：元条目卡片 -->
      <div class="bg-white rounded-8px p-16px shadow-sm border border-gray-100 flex-1 flex flex-col overflow-hidden">
        <div class="text-16px font-bold mb-16px text-gray-800 flex-shrink-0">
          元条目列表
        </div>
        <div class="flex-1 overflow-y-auto pr-4px">
          <div class="space-y-12px">
            <div
              v-for="(item, index) in infoList"
              :key="index"
              class="flex items-start justify-between gap-12px p-10px rounded-6px hover:bg-gray-50 transition-colors border-b border-gray-50 last:border-0"
            >
              <div class="text-14px text-gray-700 font-medium line-clamp-2 leading-snug flex-1">
                {{ item.title }}
              </div>
              <div class="text-12px text-gray-400 whitespace-nowrap pt-2px">
                {{ item.time }}
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
