<script setup lang="ts">
import type { FormInstance, FormRules } from "element-plus"
import type { LoginRequestData } from "@/common/apis/login/types/login"
import { reactive, ref } from "vue"
import { useRouter } from "vue-router"

import { loginApi } from "@/common/apis/login"
import { useUserStore } from "@/pinia/stores/user"

// const emit = defineEmits(["to-register"])

const router = useRouter()

const userStore = useUserStore()

/** 登录表单元素的引用 */
const loginFormRef = ref<FormInstance | null>(null)

/** 登录按钮 Loading */
const loading = ref(false)
/** 登录表单数据 */
const loginFormData: LoginRequestData = reactive({
  username: "",
  password: ""
})
/** 登录表单校验规则 */
const loginFormRules: FormRules = {
  username: [{ required: true, message: "请输入用户名", trigger: "blur" }],
  password: [
    { required: true, message: "请输入密码", trigger: "blur" },
    { min: 6, max: 16, message: "长度在 6 到 16 个字符", trigger: "blur" }
  ]
}
/** 登录逻辑 */
function handleLogin() {
  loginFormRef.value?.validate((valid: boolean, fields) => {
    if (valid) {
      loading.value = true
      // useUserStore()
      //   .login(loginFormData)
      //   .then(() => {
      //     router.push({ path: "/" })
      //   })
      //   .catch(() => {
      //     loginFormData.password = ""
      //   })
      //   .finally(() => {
      //     loading.value = false
      //   })
      loginApi(loginFormData)
        .then((res) => {
          userStore.setToken(res.data.accessToken)
          userStore.setUserName(res.data.username)
          router.push({ path: "/" })
        })
        .catch(() => {
          loginFormData.password = ""
        })
        .finally(() => {
          loading.value = false
        })
    } else {
      console.error("表单校验不通过", fields)
    }
  })
}
// 去注册页面
// function toRegister() {
//   emit("toRegister")
// }
</script>

<template>
  <el-form
    ref="loginFormRef"
    :model="loginFormData"
    :rules="loginFormRules"
    @keyup.enter="handleLogin"
    hide-required-asterisk
  >
    <h2 class="text-2xl font-bold text-center w-[100%]">
      登录
    </h2>
    <el-form-item prop="username" label="用户名" label-position="top">
      <el-input
        v-model.trim="loginFormData.username"
        placeholder="用户名"
        type="text"
        tabindex="1"
        size="large"
        clearable
      />
    </el-form-item>
    <el-form-item prop="password" label="密码" label-position="top">
      <el-input
        v-model.trim="loginFormData.password"
        placeholder="密码"
        type="password"
        tabindex="2"
        size="large"
        show-password
        clearable
      />
    </el-form-item>
    <div>
      <el-button class="w-full" :loading="loading" type="primary" size="large" @click.prevent="handleLogin">
        登 录
      </el-button>
    </div>

    <!-- <div>
      <el-button class="w-full mt-18px" size="large" @click.prevent="toRegister">注 册</el-button>
    </div> -->
  </el-form>
</template>

<style scoped></style>
