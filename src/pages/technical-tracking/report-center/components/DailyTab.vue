<script setup lang="ts">
import { FullScreen } from "@element-plus/icons-vue"
import * as echarts from "echarts"
import { onMounted, onUnmounted, reactive, ref } from "vue"

const form = reactive({
  issueNumber: "",
  dateRange: []
})

const isSubmitted = ref(true)

// 格式化输入，去除非数字字符
const formatNumber = (value: string) => value.replace(/\D/g, "")

// 限制日期最大只能选择今天
function disabledDate(time: Date) {
  return time.getTime() > Date.now()
}

// 模拟头部数据
const headerData = reactive({
  date: "2025年02月26日 · 星期三",
  currentIssue: "74",
  newsCount: 47,
  fieldCount: 8
})

const chartRef = ref<HTMLElement | null>(null)
let chartInstance: echarts.ECharts | null = null

function initChart() {
  if (!chartRef.value) return
  chartInstance = echarts.init(chartRef.value)

  const baseColors = ["#1a73e8", "#34a853", "#fb8c00"]
  const darkColors = ["#0d47a1", "#1b5e20", "#e65100"] // 加深的颜色

  let selectedPoint = { seriesIndex: -1, dataIndex: -1 }

  // 获取带选中动态样式的 series
  const getSeries = () => {
    const rawData = [
      { name: "重大部署", data: [3.4, 3.4, 1.8, 1.7, 3.4] },
      { name: "技术前沿", data: [3.4, 3.4, 3.5, 3.4, 1.7] },
      { name: "产业动态", data: [3.4, 5.2, 1.8, 1.7, 1.7] }
    ]

    return rawData.map((item, seriesIdx) => ({
      name: item.name,
      type: "bar",
      barWidth: 20,
      data: item.data,
      itemStyle: {
        borderRadius: [4, 4, 0, 0],
        color: (params: any) => {
          const isSelected = selectedPoint.seriesIndex === seriesIdx && selectedPoint.dataIndex === params.dataIndex
          return isSelected ? darkColors[seriesIdx] : baseColors[seriesIdx]
        }
      },
      emphasis: {
        disabled: true
      }
    }))
  }

  const renderOption = () => {
    const option = {
      tooltip: {
        trigger: "axis",
        axisPointer: { type: "shadow" }
      },
      legend: {
        data: ["重大部署", "技术前沿", "产业动态"],
        right: 0,
        top: 0,
        icon: "roundRect",
        itemWidth: 10,
        itemHeight: 10,
        textStyle: { color: "#666" }
      },
      grid: { left: "3%", right: "4%", bottom: "5%", top: "40px", containLabel: true },
      xAxis: {
        type: "category",
        data: ["天基太阳能", "核聚变", "量子计算", "合成生物", "商业航天"],
        axisLine: { lineStyle: { color: "#E0E6ED" } },
        axisTick: { show: false },
        axisLabel: {
          color: "#333",
          margin: 16,
          lineHeight: 20,
          rich: {
            sub: { color: "#999", fontSize: 12, align: "center" }
          }
        }
      },
      yAxis: {
        type: "value",
        axisLine: { show: false },
        axisTick: { show: false },
        splitLine: { lineStyle: { type: "solid", color: "#F0F2F5" } },
        axisLabel: { color: "#999" }
      },
      series: getSeries()
    }
    chartInstance?.setOption(option)
  }

  renderOption()

  // 添加点击事件监听
  chartInstance.off("click")
  chartInstance.on("click", (params: any) => {
    if (selectedPoint.seriesIndex === params.seriesIndex && selectedPoint.dataIndex === params.dataIndex) {
      // 再次点击已选项：反选
      selectedPoint = { seriesIndex: -1, dataIndex: -1 }
    } else {
      // 点击其他项：单选
      selectedPoint = { seriesIndex: params.seriesIndex, dataIndex: params.dataIndex }
    }
    // 更新视图
    renderOption()

    console.log("当前选中状态:", selectedPoint.seriesIndex !== -1 ? params : "已反选为空")
  })
}

function handleResize() {
  chartInstance?.resize()
}

onMounted(() => {
  if (isSubmitted.value) {
    setTimeout(() => initChart(), 0)
  }
  window.addEventListener("resize", handleResize)
})

onUnmounted(() => {
  window.removeEventListener("resize", handleResize)
  chartInstance?.dispose()
})

function handleSubmit() {
  console.log("表单提交数据:", form)
  isSubmitted.value = true
  setTimeout(() => initChart(), 0)
}

// 模拟表格数据
const tableData = reactive([
  { id: "48856", type: "技术进展", title: "无负极锂电池 | 无负极锂电池技术实现突破性进展 | 科技日报", status: "处理完成", time: "2026-03-20 00:00:00" },
  { id: "48854", type: "产业动态", title: "AI配音 | AI配音侵权维权与行业合规边界划定 | 科技日报", status: "处理完成", time: "2026-03-20 00:00:00" },
  { id: "48852", type: "技术进展", title: "具身智能 | 中国具身智能机器人实现高动态运动控制新突破 | 科技日报", status: "处理完成", time: "2026-03-20 00:00:00" },
  { id: "48851", type: "重大部署", title: "浙江省委 | 浙江省自主创新战略部署20年成效综述 | 科技日报", status: "处理完成", time: "2026-03-20 00:00:00" },
  { id: "48849", type: "技术进展", title: "土壤监测 | 基于分布式光纤传感的农田土壤水分实时监测技术取得突破 | 科技日报", status: "处理完成", time: "2026-03-20 00:00:00" },
  { id: "48847", type: "重大部署", title: "党中央 | 十五五规划纲要制定与实施部署 | 科技日报", status: "处理完成", time: "2026-03-20 00:00:00" },
  { id: "48848", type: "技术进展", title: "燃气轮机 | 我国3兆瓦级CGT3燃气轮机实现自主可控技术突破 | 科技日报", status: "处理完成", time: "2026-03-20 00:00:00" },
  { id: "48846", type: "产业动态", title: "集成电路 | 新一代集成电路制造工艺与装备产业化项目启动 | 科技日报", status: "处理完成", time: "2026-03-19 00:00:00" },
  { id: "48845", type: "技术进展", title: "量子通信 | 超导量子计算原型机实现百比特纠缠 | 科技日报", status: "处理完成", time: "2026-03-19 00:00:00" }
])

const dialogVisible = ref(false)
const editForm = reactive({
  title: "",
  keyword: "",
  type: "",
  content: ""
})

function handleEdit(row: any) {
  const parts = row.title.split(" | ")
  editForm.title = parts[1] || row.title
  editForm.keyword = parts[0] || row.keyword || ""
  editForm.type = row.type
  editForm.content = `习近平于3月23日在河北雄安新区考察，主持召开深入推进雄安新区高质量建设和发展座谈会并发表重要讲话。会议强调必须牢牢把握雄安新区功能定位，努力建设新时代创新高地和推动高质量发展样板。此举为雄安新区打造科技创新引擎和区域协同发展示范提供了战略支撑。`
  dialogVisible.value = true
}

function handleRewrite() {
  console.log("AI重写", editForm)
}

function handleSave() {
  console.log("确认保存", editForm)
  dialogVisible.value = false
}
</script>

<template>
  <div class="h-full flex flex-col">
    <!-- 头部搜索表单 -->
    <div v-if="!isSubmitted" class="py-32px">
      <el-form :model="form" @submit.prevent>
        <div class="flex justify-center gap-24px mb-24px">
          <el-form-item label="期数" class="!mb-0 w-300px">
            <el-input
              v-model="form.issueNumber"
              placeholder="请输入期数"
              :formatter="formatNumber"
              :parser="formatNumber"
              clearable
            />
          </el-form-item>

          <el-form-item label="日期区间" class="!mb-0">
            <el-date-picker
              v-model="form.dateRange"
              type="daterange"
              range-separator="至"
              start-placeholder="开始日期"
              end-placeholder="结束日期"
              value-format="YYYY-MM-DD"
              :disabled-date="disabledDate"
              clearable
            />
          </el-form-item>
        </div>

        <div class="flex justify-center">
          <el-button type="primary" size="large" class="px-48px text-16px" @click="handleSubmit">
            确认
          </el-button>
        </div>
      </el-form>
    </div>

    <!-- 内部内容滚动区 -->
    <div v-if="isSubmitted" class="flex-1 overflow-y-auto pb-24px flex flex-col gap-16px">
      <!-- 头部数据卡片 -->
      <div class="relative bg-[#1a73e8] rounded-12px px-24px py-16px text-white overflow-hidden shrink-0">
        <div class="absolute -right-[100px] -top-[100px] w-[300px] h-[300px] bg-white opacity-10 rounded-full pointer-events-none" />

        <div class="flex items-center justify-between">
          <div>
            <div class="flex items-center gap-12px mb-16px text-12px font-medium">
              <div class="px-16px py-6px bg-white/20 rounded-full backdrop-blur-sm">
                {{ headerData.date }}
              </div>
              <div class="px-16px py-6px bg-[#409EFF] rounded-full shadow-sm">
                第 {{ headerData.currentIssue }} 期
              </div>
            </div>

            <div class="text-20px font-bold tracking-widest">
              每日科技动态
            </div>
          </div>

          <div class="flex items-center gap-48px mr-48px">
            <div class="flex flex-col gap-4px items-center">
              <div class="text-28px font-bold leading-none">
                {{ headerData.newsCount }}
              </div>
              <div class="text-12px text-white/80">
                今日收录资讯
              </div>
            </div>
            <div class="flex flex-col gap-4px items-center">
              <div class="text-28px font-bold leading-none">
                {{ headerData.fieldCount }}
              </div>
              <div class="text-12px text-white/80">
                覆盖技术领域
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- 领域资讯分布 -->
      <div class="bg-white border-solid border border-gray-100 rounded-12px p-16px pb-0 shadow-sm shrink-0">
        <div class="flex items-center gap-8px mb-16px">
          <div class="w-4px h-16px bg-[#1a73e8] rounded-2px" />
          <div class="text-16px font-bold text-gray-800">
            领域资讯分布
          </div>
        </div>
        <div class="h-200px w-full" ref="chartRef" />
      </div>

      <!-- 资讯列表 -->
      <div class="bg-white border-solid border border-gray-100 rounded-12px p-24px shadow-sm flex flex-col gap-16px flex-1 min-h-500px">
        <div class="flex items-center justify-between">
          <div class="flex items-center gap-8px">
            <div class="w-4px h-16px bg-[#1a73e8] rounded-2px" />
            <div class="text-16px font-bold text-gray-800">
              资讯列表
            </div>
            <div class="text-12px text-gray-500 ml-8px">
              请从下方资讯列表中选择待处理资讯
            </div>
          </div>
          <div class="flex items-center gap-12px">
            <el-button type="primary">
              分条纳入
            </el-button>
            <el-button type="primary">
              多条聚合
            </el-button>
          </div>
        </div>

        <div class="flex-1 overflow-hidden">
          <el-table :data="tableData" height="100%" stripe class="w-full">
            <el-table-column type="selection" width="55" align="center" />
            <el-table-column label="操作" width="100" align="center">
              <template #default="scope">
                <el-button link type="primary" size="small" @click="handleEdit(scope.row)">
                  编辑
                </el-button>
              </template>
            </el-table-column>
            <el-table-column prop="id" label="新闻ID" width="100" align="center" />
            <el-table-column prop="type" label="新闻类型" width="120" align="center" />
            <el-table-column prop="title" label="新闻标题" min-width="400" show-overflow-tooltip>
              <template #default="scope">
                <span
                  :class="{
                    'text-[#fb8c00]': scope.row.type === '产业动态',
                    'text-[#34a853]': scope.row.type === '技术进展',
                    'text-[#1a73e8]': scope.row.type === '重大部署',
                  }"
                >{{ scope.row.title.split(' | ')[0] }}</span>
                <span class="text-gray-300 mx-4px">|</span>
                <span>{{ scope.row.title.split(' | ')[1] }}</span>
                <span class="text-gray-300 mx-4px">|</span>
                <span class="text-gray-500">{{ scope.row.title.split(' | ')[2] }}</span>
              </template>
            </el-table-column>
            <el-table-column label="新闻外链" width="100" align="center">
              <template #default>
                <el-button link type="primary">
                  点击查看
                </el-button>
              </template>
            </el-table-column>
            <el-table-column prop="status" label="新闻状态" width="100" align="center" />
            <el-table-column prop="time" label="新闻时间" width="160" align="center" />
          </el-table>
        </div>
      </div>
    </div>

    <!-- 编辑新闻弹窗 -->
    <el-dialog v-model="dialogVisible" title="编辑新闻" width="600px" destroy-on-close align-center>
      <el-form :model="editForm" label-width="100px" class="pr-24px">
        <el-form-item label="新闻标题" required>
          <div class="flex items-center gap-12px w-full">
            <el-input v-model="editForm.title" class="flex-1" />
            <el-link type="primary" :underline="false">
              新闻外链
            </el-link>
          </div>
        </el-form-item>

        <el-form-item label="新闻关键字" required>
          <el-input v-model="editForm.keyword" />
        </el-form-item>

        <el-form-item label="新闻类型" required>
          <el-select v-model="editForm.type" class="w-full">
            <el-option label="重大部署" value="重大部署" />
            <el-option label="技术进展" value="技术进展" />
            <el-option label="产业动态" value="产业动态" />
          </el-select>
        </el-form-item>

        <el-form-item label="简报内容" required>
          <div class="flex flex-col w-full">
            <el-input
              v-model="editForm.content"
              type="textarea"
              :rows="8"
              resize="none"
              class="bg-gray-50 bg-opacity-50"
            />
          </div>
        </el-form-item>
      </el-form>
      <template #footer>
        <div class="flex justify-center gap-16px pb-16px">
          <el-button type="warning" @click="handleRewrite" class="px-24px">
            AI重写
          </el-button>
          <el-button type="primary" @click="handleSave" class="px-24px">
            确认
          </el-button>
        </div>
      </template>
    </el-dialog>
  </div>
</template>

<style scoped lang="scss"></style>
