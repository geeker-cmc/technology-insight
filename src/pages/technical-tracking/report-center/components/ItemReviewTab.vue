<script setup lang="ts">
import { ArrowDown, Cpu, Delete, Download, Lightning, Odometer, Refresh, TopRight } from "@element-plus/icons-vue"
// @ts-expect-error missing types
import { Editor, Toolbar } from "@wangeditor/editor-for-vue"
import { computed, onBeforeUnmount, ref, shallowRef } from "vue"
import "@wangeditor/editor/dist/css/style.css"

const themes = [
  { label: "重大部署", icon: Cpu, color: "bg-[#d32f2f]" },
  { label: "技术前沿", icon: Odometer, color: "bg-[#8e24aa]" },
  { label: "产业动态", icon: Lightning, color: "bg-[#1a73e8]" }
]

const selectedTheme = ref<string | null>(null)

// 切换主题，若已选中则反选（设为null即为全部）
function toggleTheme(theme: string) {
  if (selectedTheme.value === theme) {
    selectedTheme.value = null
  } else {
    selectedTheme.value = theme
  }
}

// 模拟左侧列表数据
const mockData = [
  { id: 1, type: "重大部署", title: "条目一、国务院部署央企AI大模型示范场景：2026年底前须完成5个以上应用场景", total: 6, selected: 3 },
  { id: 2, type: "技术前沿", title: "条目二、SPARC装置完成首次等离子体点火，可控核聚变商业化时间表提前至2035", total: 5, selected: 3 },
  { id: 3, type: "产业动态", title: "条目三、南方电网配网智能化三年行动计划发布，大湾区超600亿投入拉动智能装备", total: 5, selected: 3 }
]

const activeItemId = ref(1)

// 右侧来源 mock 数据
const sourceList = ref([
  { id: 1, checked: true, title: "国务院常务会议：央企须完成5个以上AI大模型示范场景，国资委牵头制定评价体系", summary: "会议强调充分发挥央企在AI技术攻关和场景应用中的压舱石作用，国资委将制定量化评估指标，与央企负责人考核挂钩，对拥有算力基础设施的央企将优先纳入示范评价范围。" },
  { id: 2, checked: true, title: "国务院常务会议：央企须完成5个以上AI大模型示范场景，国资委牵头制定评价体系", summary: "会议强调充分发挥央企在AI技术攻关和场景应用中的压舱石作用，国资委将制定量化评估指标，与央企负责人考核挂钩，对拥有算力基础设施的央企将优先纳入示范评价范围。" },
  { id: 3, checked: true, title: "国务院常务会议：央企须完成5个以上AI大模型示范场景，国资委牵头制定评价体系", summary: "会议强调充分发挥央企在AI技术攻关和场景应用中的压舱石作用，国资委将制定量化评估指标，与央企负责人考核挂钩，对拥有算力基础设施的央企将优先纳入示范评价范围。" },
  { id: 4, checked: true, title: "国务院常务会议：央企须完成5个以上AI大模型示范场景，国资委牵头制定评价体系", summary: "会议强调充分发挥央企在AI技术攻关和场景应用中的压舱石作用，国资委将制定量化评估指标，与央企负责人考核挂钩，对拥有算力基础设施的央企将优先纳入示范评价范围。" }
])

// 根据选择的主题过滤并分组
const displayGroups = computed(() => {
  let filtered = mockData
  // 有选择则过滤，没选则保留全部
  if (selectedTheme.value) {
    filtered = mockData.filter(item => item.type === selectedTheme.value)
  }

  const groups: any[] = []
  themes.forEach((theme) => {
    const items = filtered.filter(item => item.type === theme.label)
    if (items.length > 0) {
      groups.push({
        label: theme.label,
        color: theme.color,
        items
      })
    }
  })
  return groups
})

// 预览抽屉状态和数据
const previewVisible = ref(false)
const richTextContent = ref(`
  <div style="font-family: sans-serif; line-height: 1.8; color: #333;">
    <h3 style="color: #1a73e8; margin-bottom: 12px; border-bottom: 1px solid #eee; padding-bottom: 8px;">一、重大部署</h3>
    <p style="margin-bottom: 16px;"><strong>条目一、国务院部署央企AI大模型示范场景，2026年底前须完成5个以上应用场景</strong></p>
    <p style="margin-bottom: 24px; text-indent: 2em; color: #555;">近期，国务院在推动“人工智能+”行动的相关部署中提出，要加快中央企业在AI大模型示范应用场景上的落地建设，推动人工智能与实体产业深度融合。根据部署要求，中央企业需围绕能源、电力、制造、交通、金融等重点领域...</p>
    
    <h3 style="color: #1a73e8; margin-bottom: 12px; border-bottom: 1px solid #eee; padding-bottom: 8px;">二、技术前沿</h3>
    <p style="margin-bottom: 16px;"><strong>条目二、SPARC装置完成首次等离子体点火，可控核聚变商业化时间表提前至2035</strong></p>
    <p style="margin-bottom: 24px; text-indent: 2em; color: #555;">在清洁能源技术领域取得突破性进展，SPARC装置近日成功实现首次等离子体点火，标志着人类向可控核聚变商业化迈出了关键一步。专家预计，这一里程碑事件将使原定的商业化时间表大幅提前...</p>
  </div>
`)

// 富文本编辑器配置
const editorRef = shallowRef()
const mode = "default"
const toolbarConfig: any = {
  excludeKeys: [
    "group-video",
    "insert-video",
    "upload-video",
    "group-image",
    "insert-image",
    "upload-image",
    "insertTable",
    "deleteTable",
    "insertTableRow",
    "deleteTableRow",
    "insertTableCol",
    "deleteTableCol",
    "tableHeader",
    "tableFullWidth",
    "group-more-style",
    "emotion",
    "insertCode",
    "codeBlock",
    "fullScreen"
  ]
}
const editorConfig = {
  placeholder: "请输入内容...",
  MENU_CONF: {
    uploadImage: {
      // 禁用上传图片，或者您可以配置 server 接口
      // server: '/api/upload',
    }
  }
}

function handleCreated(editor: any) {
  editorRef.value = editor
}

onBeforeUnmount(() => {
  const editor = editorRef.value
  if (editor == null) return
  editor.destroy()
})
</script>

<template>
  <div class="w-full h-full flex flex-1 overflow-hidden border border-solid border-gray-100 rounded-12px">
    <!-- 左侧列表 -->
    <div class="w-[340px] border-r border-solid border-gray-200 flex flex-col shrink-0 bg-white">
      <div class="px-16px py-16px border-b border-solid border-gray-100 flex items-center justify-between">
        <span class="text-16px font-bold text-gray-500 tracking-wider">聚类主题</span>
        <el-button type="primary" size="small" class="!px-16px" @click="previewVisible = true">
          预览
        </el-button>
      </div>

      <!-- 筛选按钮区（去掉了全部，三种单选/反选） -->
      <div class="p-16px py-12px border-b border-solid border-gray-100 flex gap-12px flex-wrap">
        <div
          v-for="theme in themes"
          :key="theme.label"
          class="flex items-center gap-6px px-12px py-6px rounded-6px border border-solid cursor-pointer text-14px select-none"
          :class="selectedTheme === theme.label ? 'border-[#1a73e8] bg-[#e8f0fe] text-[#1a73e8] font-medium' : 'border-gray-200 text-gray-600 bg-white'"
          @click="toggleTheme(theme.label)"
        >
          {{ theme.label }}
        </div>
      </div>

      <!-- 条目列表区域 -->
      <div class="flex-1 overflow-y-auto flex flex-col gap-12px pb-16px">
        <div v-for="group in displayGroups" :key="group.label" class="flex flex-col">
          <!-- 分组标题 -->
          <div class="text-13px text-gray-400 px-16px pt-16px pb-8px font-medium">
            {{ group.label }}
          </div>

          <!-- 组内项 -->
          <div class="flex flex-col gap-8px px-12px">
            <div
              v-for="item in group.items"
              :key="item.id"
              class="relative p-12px pl-16px rounded-8px cursor-pointer flex gap-12px"
              :class="activeItemId === item.id ? 'bg-[#edf3fd]' : 'bg-transparent'"
              @click="activeItemId = item.id"
            >
              <!-- 激活红条 -->
              <div v-if="activeItemId === item.id" class="absolute left-0 top-12px bottom-12px w-4px bg-[#d32f2f] rounded-r-4px" />

              <!-- 删除图标（按照要求去除了悬停样式） -->
              <div class="mt-2px shrink-0 text-gray-600 cursor-pointer">
                <el-icon class="text-16px">
                  <Delete />
                </el-icon>
              </div>

              <!-- 右侧详情文字 -->
              <div class="flex flex-col gap-8px flex-1">
                <div class="text-15px font-bold leading-snug" :class="activeItemId === item.id ? 'text-[#1a73e8]' : 'text-gray-800'">
                  {{ item.title }}
                </div>
                <div class="flex items-center gap-16px text-12px mt-4px">
                  <div class="flex items-center gap-6px text-gray-400">
                    <div class="w-6px h-6px rounded-full" :class="group.color" />
                    {{ item.total }}条
                  </div>
                  <div class="text-[#fb8c00] font-medium">
                    已选{{ item.selected }}/{{ item.total }}
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- 右侧内容详情 -->
    <div class="flex-1 bg-white flex flex-col overflow-hidden">
      <div v-if="activeItemId" class="flex-1 flex flex-col h-full overflow-hidden p-24px pr-32px pb-0 gap-16px">
        <!-- 标签和标题 -->
        <div class="flex flex-col gap-12px shrink-0">
          <div class="flex items-center gap-8px text-12px text-[#d32f2f]">
            <div class="w-6px h-6px rounded-full bg-[#d32f2f]" />
            <span class="font-medium">重大部署</span>
          </div>
          <div class="text-18px font-bold text-gray-900 leading-snug">
            条目一、国务院部署央企AI大模型示范场景，2026年底前须完成5个以上应用场景
          </div>
        </div>

        <!-- 总结正文（带边框的高亮盒子） -->
        <div class="p-20px border-2 border-solid border-[#1a73e8] bg-white rounded-4px text-14px text-gray-800 leading-relaxed tracking-wide shadow-sm mt-4px shrink-0">
          近期，国务院在推动“人工智能+”行动的相关部署中提出，要加快中央企业在AI大模型示范应用场景上的落地建设，推动人工智能与实体产业深度融合。根据部署要求，中央企业需围绕能源、电力、制造、交通、金融等重点领域，系统推进大模型在生产优化、设备运维、知识管理、智能决策和客户服务等环节的应用示范，并形成可复制、可推广的解决方案。政策明确提出，到2026年底前，重点中央企业原则上需完成不少于5个具有行业代表性的AI大模型应用场景建设，通过示范项目带动产业链上下游数字化和智能化升级。同时，国资委将加强统筹协调和评估机制，推动央企在算力基础设施、行业数据资源和模型应用生态方面形成协同发展格局，进一步提升国家战略科技力量与产业智能化水平。
        </div>

        <!-- 来源资讯及按钮栏 -->
        <div class="flex items-center justify-between mt-12px mb-4px shrink-0">
          <div class="flex items-center gap-8px text-[#34a853] font-bold text-16px tracking-wider">
            <el-icon class="text-18px font-bold">
              <Download />
            </el-icon>
            来源资讯
          </div>
          <el-button type="primary" :icon="Refresh" class="!px-16px !rounded-6px shadow-sm">
            AI重新生成
          </el-button>
        </div>

        <!-- 来源列表 (折叠面板风格) -->
        <div class="flex-1 overflow-y-auto flex flex-col gap-16px pb-24px pr-8px">
          <div
            v-for="(source) in sourceList"
            :key="source.id"
            class="flex border border-solid border-[#a0c3ff] rounded-8px overflow-hidden bg-white shadow-sm"
          >
            <!-- 卡片左侧红条边框 -->
            <div class="w-4px shrink-0 bg-[#d32f2f]" />
            <!-- 卡片主体内容 -->
            <div class="flex-1 p-16px pr-20px flex flex-col gap-12px">
              <div class="flex items-start justify-between">
                <!-- 复选框及标题正文组合 -->
                <div class="flex items-start gap-12px">
                  <el-checkbox v-model="source.checked" class="mt-2px" />
                  <div class="flex flex-col gap-8px">
                    <div class="text-15px font-bold text-gray-900 leading-tight pr-24px">
                      {{ source.title }}
                    </div>
                    <div class="flex gap-12px">
                      <!-- 摘要内容 -->
                      <span class="text-13px text-gray-500 leading-relaxed pr-16px text-justify">
                        {{ source.summary }}
                      </span>
                    </div>
                  </div>
                </div>
                <!-- 展开箭头 -->
                <el-icon class="text-gray-400 cursor-pointer font-bold shrink-0 mt-4px">
                  <ArrowDown />
                </el-icon>
              </div>

              <!-- 查看原文按钮 -->
              <div class="ml-36px">
                <el-button link type="primary" class="!text-13px !h-auto !py-0">
                  <el-icon class="mr-4px">
                    <TopRight />
                  </el-icon>查看原文
                </el-button>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- 未选中任何条目时的空状态 -->
      <div v-else class="flex-1 flex items-center justify-center">
        <div class="text-gray-400">
          请选择左侧条目查看详细信息
        </div>
      </div>
    </div>

    <!-- 预览抽屉 -->
    <el-drawer
      v-model="previewVisible"
      title="日报第2期"
      size="1200px"
      append-to-body
      destroy-on-close
    >
      <div class="h-full flex flex-col pt-8px">
        <!-- 抽屉主体：富文本容器 -->
        <div class="flex-1 overflow-hidden w-full flex flex-col border border-solid border-gray-200 rounded-8px">
          <!-- 工具栏 -->
          <Toolbar
            :editor="editorRef"
            :default-config="toolbarConfig"
            :mode="mode"
            class="border-b border-solid border-gray-200 shrink-0"
          />
          <!-- 编辑器主体 -->
          <Editor
            v-model="richTextContent"
            :default-config="editorConfig"
            :mode="mode"
            class="flex-1 overflow-hidden"
            @on-created="handleCreated"
          />
        </div>
      </div>
      <!-- 底部操作按钮 -->
      <template #footer>
        <div class="flex justify-end gap-12px pr-8px">
          <el-button @click="previewVisible = false">
            保存草稿
          </el-button>
          <el-button type="primary" @click="previewVisible = false">
            下载并归档
          </el-button>
        </div>
      </template>
    </el-drawer>
  </div>
</template>

<style scoped lang="scss"></style>
