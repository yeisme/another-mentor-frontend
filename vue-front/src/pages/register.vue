<template>
  <div class="register-container">
    <el-card class="register-card">
      <h2>用户注册</h2>
      <el-form :model="registerForm" :rules="rules" ref="registerFormRef" label-width="100px">
        <el-form-item label="用户名" prop="username">
          <el-input v-model="registerForm.username" placeholder="请输入用户名"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input type="password" v-model="registerForm.password" placeholder="请输入密码"></el-input>
        </el-form-item>
        <el-form-item label="确认密码" prop="confirmPassword">
          <el-input type="password" v-model="registerForm.confirmPassword" placeholder="请再次输入密码"></el-input>
        </el-form-item>
        <el-form-item label="邮箱" prop="email">
          <el-input v-model="registerForm.email" placeholder="请输入邮箱"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm">注册</el-button>
          <el-button @click="resetForm">重置</el-button>
        </el-form-item>
      </el-form>
    </el-card>
  </div>
</template>

<script>
import { ref } from 'vue';
import { ElMessage } from 'element-plus';

export default {
  setup() {
    const registerFormRef = ref(null);
    const registerForm = ref({
      username: '',
      password: '',
      confirmPassword: '',
      email: ''
    });

    const validateConfirmPassword = (rule, value, callback) => {
      if (value !== registerForm.value.password) {
        callback(new Error('两次输入的密码不一致'));
      } else {
        callback();
      }
    };

    const rules = {
      username: [
        { required: true, message: '请输入用户名', trigger: 'blur' },
        { min: 3, max: 10, message: '用户名长度在 3 到 10 个字符', trigger: 'blur' }
      ],
      password: [
        { required: true, message: '请输入密码', trigger: 'blur' },
        { min: 6, max: 16, message: '密码长度在 6 到 16 个字符', trigger: 'blur' }
      ],
      confirmPassword: [
        { required: true, message: '请再次输入密码', trigger: 'blur' },
        { validator: validateConfirmPassword, trigger: 'blur' }
      ],
      email: [
        { required: true, message: '请输入邮箱', trigger: 'blur' },
        { type: 'email', message: '请输入正确的邮箱地址', trigger: ['blur', 'change'] }
      ]
    };

    const submitForm = () => {
      registerFormRef.value.validate((valid) => {
        if (valid) {
          // 这里可以调用注册接口
          ElMessage.success('注册成功！');
          console.log('注册信息:', registerForm.value);
        } else {
          ElMessage.error('请检查表单填写是否正确');
          return false;
        }
      });
    };

    const resetForm = () => {
      registerFormRef.value.resetFields();
    };

    return {
      registerFormRef,
      registerForm,
      rules,
      submitForm,
      resetForm
    };
  }
};
</script>

<style scoped>
.register-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f5f5f5;
}

.register-card {
  width: 400px;
  padding: 20px;
}

h2 {
  color:#673ab7;
  text-align: center;
  margin-bottom: 20px;
}
</style>