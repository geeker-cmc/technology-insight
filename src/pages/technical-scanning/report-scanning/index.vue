<script setup lang="ts">
import { ArrowRight, Files, Operation, Search, Timer } from "@element-plus/icons-vue"
import { ref } from "vue"

const researchObject = ref("量子科技")
const subObject = ref("")
const timeWindow = ref("5")

function formatDate(date: Date) {
  const y = date.getFullYear()
  const m = String(date.getMonth() + 1).padStart(2, "0")
  const d = String(date.getDate()).padStart(2, "0")
  return `${y}-${m}-${d}`
}

const today = new Date()
const dateRange = ref<[string, string]>([
  formatDate(new Date(today.getFullYear() - 5, today.getMonth(), today.getDate())),
  formatDate(today)
])

function handleTimeWindowChange(val: string | number | boolean) {
  const end = new Date()
  const start = new Date()
  if (val === "5") {
    start.setFullYear(end.getFullYear() - 5)
    dateRange.value = [formatDate(start), formatDate(end)]
  } else if (val === "10") {
    start.setFullYear(end.getFullYear() - 10)
    dateRange.value = [formatDate(start), formatDate(end)]
  }
}

function handleDateChange() {
  timeWindow.value = ""
}

const dataSources = ref(["WOS", "arXiv", "patent"])
</script>

<template>
  <div class="app-container h-full w-full bg-[#F5F7FA] p-16px overflow-y-auto">
    <!-- 中间部分居中：通过 max-w 和 mx-auto 实现 -->
    <main class="max-w-1200px mx-auto bg-white rounded-8px shadow-sm min-h-full p-40px">
      <!-- 条件选择区域 -->
      <div class="border-1px border-[#E4E7ED] border-dashed rounded-8px p-32px mb-40px bg-[#FCFDFE]">
        <el-form label-position="top">
          <el-row :gutter="64">
            <!-- 第一行：研究对象 & 子对象 -->
            <el-col :span="12">
              <el-form-item required class="!mb-0">
                <template #label>
                  <div class="flex items-center gap-8px mb-4px inline-flex">
                    <span class="font-600 text-15px text-#303133">研究对象</span>
                  </div>
                </template>
                <el-input v-model="researchObject" placeholder="请输入研究对象" size="large" class="w-full" />
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item class="!mb-0">
                <template #label>
                  <div class="flex items-center gap-8px mb-4px inline-flex">
                    <span class="font-600 text-15px text-#303133">子对象 (可选)</span>
                  </div>
                </template>
                <el-input v-model="subObject" placeholder="例：量子通信 / 量子计算" size="large" class="w-full" />
              </el-form-item>
            </el-col>

            <!-- 第二行：时间窗口 -->
            <el-col :span="24" class="mt-24px">
              <el-form-item class="!mb-0">
                <template #label>
                  <div class="flex items-center gap-8px mb-4px">
                    <span class="font-600 text-15px text-#303133">时间窗口</span>
                  </div>
                </template>
                <div class="flex items-center gap-12px">
                  <el-radio-group v-model="timeWindow" size="large" @change="handleTimeWindowChange">
                    <el-radio-button label="5">
                      近5年
                    </el-radio-button>
                    <el-radio-button label="10">
                      近10年
                    </el-radio-button>
                  </el-radio-group>
                  <div class="flex items-center gap-8px ml-4px">
                    <el-date-picker
                      v-model="dateRange"
                      type="daterange"
                      range-separator="至"
                      start-placeholder="开始日期"
                      end-placeholder="结束日期"
                      format="YYYY-MM-DD"
                      value-format="YYYY-MM-DD"
                      size="large"
                      class="!w-320px"
                      @change="handleDateChange"
                    />
                  </div>
                </div>
              </el-form-item>
            </el-col>

            <!-- 第三行：数据源选择 -->
            <el-col :span="24" class="mt-24px">
              <el-form-item class="!mb-0">
                <template #label>
                  <div class="flex items-center gap-8px mb-4px">
                    <span class="font-600 text-15px text-#303133">数据源选择</span>
                  </div>
                </template>
                <el-checkbox-group v-model="dataSources" size="large" class="flex gap-16px">
                  <el-checkbox label="WOS">
                    WOS 核心合集
                  </el-checkbox>
                  <el-checkbox label="arXiv">
                    arXiv
                  </el-checkbox>
                  <el-checkbox label="patent">
                    专利
                  </el-checkbox>
                </el-checkbox-group>
              </el-form-item>
            </el-col>
          </el-row>
        </el-form>
      </div>

      <!-- 开始研究扫描按钮 -->
      <div class="flex justify-center mt-60px">
        <el-button type="primary" size="large" class="!px-60px !py-28px !text-20px !rounded-full shadow-lg">
          开始研究扫描
        </el-button>
      </div>
    </main>
  </div>
</template>

<style scoped lang="scss">
/* 隐藏滚动条但保留功能 */
.overflow-y-auto {
  -ms-overflow-style: none;
  scrollbar-width: none;
  &::-webkit-scrollbar {
    display: none;
  }
}

:deep(.el-form-item__label) {
  padding-bottom: 0 !important;
}
</style>
