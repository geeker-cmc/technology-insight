<script setup lang="ts">
import { DomEditor } from "@wangeditor/editor"
// @ts-expect-error missing types
import { Editor, Toolbar } from "@wangeditor/editor-for-vue"
import { onBeforeUnmount, ref, shallowRef } from "vue"
import "@wangeditor/editor/dist/css/style.css"

/** 章节结构数据模拟 */
const sectionList = ref([
  { title: "一、执行摘要", words: 280, status: "完成" },
  { title: "二、重大部署", words: 240, status: "完成" },
  { title: "三、技术前沿", words: 860, status: "完成" },
  { title: "四、产业动态", words: 480, status: "完成" }
])

// 编辑器实例，必须用 shallowRef
const editorRef = shallowRef()

// 内容 HTML
const valueHtml = ref("")

// 输出内容的变化
function handleChange(editor: any) {
  console.log("当前富文本内容 (HTML):", editor.getHtml())
  console.log("当前富文本内容 (Text):", editor.getText())
}

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

// 组件销毁时，也及时销毁编辑器
onBeforeUnmount(() => {
  const editor = editorRef.value
  if (editor == null) return
  editor.destroy()
})

function handleCreated(editor: any) {
  editorRef.value = editor // 记录 editor 实例，重要！

  // 输出所有 toolbarKeys
  const toolbar = DomEditor.getToolbar(editor)
  if (toolbar) {
    console.log("当前 Toolbar 的所有 Keys:", toolbar.getConfig().toolbarKeys)
  }
}
</script>

<template>
  <div class="app-container flex h-full w-full overflow-hidden bg-[var(--v3-body-bg-color)] p-16px gap-16px">
    <!-- 中间：富文本编辑器 -->
    <main class="flex-1 min-w-0 bg-white rounded-8px shadow-sm flex flex-col overflow-hidden">
      <div class="p-20px font-bold text-18px border-b border-gray-100 flex-shrink-0">
        编辑报告内容
      </div>

      <!-- wangEditor 区域 -->
      <div class="flex-1 flex flex-col overflow-hidden min-h-0 border-t border-gray-100 p-10px">
        <Toolbar
          class="border-b border-gray-100"
          :editor="editorRef"
          :default-config="toolbarConfig"
          mode="default"
        />
        <Editor
          v-model="valueHtml"
          class="flex-1 overflow-y-hidden"
          style="overflow-y: hidden;"
          :default-config="editorConfig"
          mode="default"
          @on-created="handleCreated"
          @on-change="handleChange"
        />
      </div>
    </main>

    <!-- 右侧：章节结构与操作 -->
    <aside class="w-300px flex-shrink-0 flex flex-col gap-16px overflow-hidden">
      <!-- 专报章节结构卡片 -->
      <div class="bg-white rounded-12px p-20px shadow-sm border border-gray-100">
        <div class="flex items-center gap-8px mb-20px">
          <span class="text-16px font-bold text-gray-800">专报章节结构</span>
        </div>

        <div class="space-y-16px">
          <div v-for="(item, index) in sectionList" :key="index" class="flex items-center justify-between">
            <div class="flex items-center gap-10px">
              <span class="w-8px h-8px rounded-full bg-green-500" />
              <span class="text-15px text-gray-700">{{ item.title }}</span>
            </div>
            <div class="flex items-center gap-12px">
              <span class="text-12px text-gray-400">约{{ item.words }}字</span>
              <span class="bg-green-50 text-green-600 text-12px px-8px py-3px rounded-4px font-medium">
                {{ item.status }}
              </span>
            </div>
          </div>
        </div>
      </div>

      <!-- 操作按钮区域 -->
      <div class="mt-auto">
        <el-button type="primary" class="!w-full !h-48px !text-16px !rounded-8px flex items-center justify-center gap-8px">
          下载并归档报告
        </el-button>
      </div>
    </aside>
  </div>
</template>

<style scoped lang="scss">
/* 保持隐藏滚动条样式 */
.overflow-y-auto {
  -ms-overflow-style: none;
  scrollbar-width: none;
  &::-webkit-scrollbar {
    display: none;
  }
}
</style>
