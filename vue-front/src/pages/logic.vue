<template>
  <v-container
    class="fill-height"
    fluid
  >
    <v-row
      align="center"
      justify="center"
    >
      <v-col
        cols="12"
        sm="8"
        md="6"
        lg="4"
      >
        <v-card class="elevation-12 pa-4">
          <v-card-title class="text-h5 font-weight-bold text-center">
            用户登录
          </v-card-title>
          <v-card-text>
            <v-form
              ref="loginForm"
              @submit.prevent="handleLogin"
            >
              <v-text-field
                v-model="loginForm.account"
                label="账号/邮箱"
                prepend-icon="mdi-account"
                variant="outlined"
                :rules="[v => !!v || '账号不能为空']"
                required
              />

              <v-text-field
                v-model="loginForm.password"
                label="密码"
                prepend-icon="mdi-lock"
                variant="outlined"
                type="password"
                :rules="[v => !!v || '密码不能为空']"
                required
              />

              <div
                v-if="errorMessage"
                class="text-error mb-4"
              >
                {{ errorMessage }}
              </div>

              <v-btn
                color="primary"
                block
                type="submit"
                :loading="loading"
                class="mt-2"
              >
                登录
              </v-btn>

              <div class="d-flex justify-space-between mt-4">
                <v-btn
                  variant="text"
                  @click="router.push('/register')"
                >
                  注册账号
                </v-btn>
                <v-btn variant="text">
                  忘记密码
                </v-btn>
              </div>
            </v-form>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const loading = ref(false)
const errorMessage = ref('')
const loginForm = reactive({
  account: '',
  password: ''
})

const handleLogin = async () => {
  loading.value = true
  errorMessage.value = ''

    // TODO 从注册中心获取后端 user 服务
  try {
    const response = await fetch('http://localhost:8080/api/v1/user/login', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
      },
      body: JSON.stringify(loginForm)
    })

    const data = await response.json()

    if (!response.ok) {
      throw new Error(data.message || '登录失败')
    }

    // 存储用户信息和令牌
    localStorage.setItem('token', data.token || '')
    localStorage.setItem('user', JSON.stringify(data.user || {}))

    // 登录成功后跳转
    router.push('/home')
  } catch (error) {
    console.error('登录错误:', error)
    errorMessage.value = error instanceof Error ? error.message : '登录过程中发生错误'
  } finally {
    loading.value = false
  }
}
</script>

<style scoped>
.text-error {
  color: #ff5252;
  font-size: 0.875rem;
}
</style>
