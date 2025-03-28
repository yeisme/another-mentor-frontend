<template>
  <div class="reset-password-container">
    <el-card class="reset-password-card">
      <h2>重置密码</h2>
      <el-form :model="resetForm" :rules="rules" ref="resetFormRef" label-width="120px">
        <el-form-item label="新密码" prop="newPassword">
          <el-input type="password" v-model="resetForm.newPassword" placeholder="请输入新密码"></el-input>
        </el-form-item>
        <el-form-item label="确认新密码" prop="confirmPassword">
          <el-input type="password" v-model="resetForm.confirmPassword" placeholder="请再次输入新密码"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm">提交</el-button>
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
    const resetFormRef = ref(null);
    const resetForm = ref({
      newPassword: '',
      confirmPassword: ''
    });

    // 验证规则
    const validateConfirmPassword = (rule, value, callback) => {
      if (value !== resetForm.value.newPassword) {
        callback(new Error('两次输入的密码不一致'));
      } else {
        callback();
      }
    };

    const rules = {
      newPassword: [
        { required: true, message: '请输入新密码', trigger: 'blur' },
        { min: 6, max: 16, message: '密码长度在 6 到 16 个字符', trigger: 'blur' }
      ],
      confirmPassword: [
        { required: true, message: '请再次输入新密码', trigger: 'blur' },
        { validator: validateConfirmPassword, trigger: 'blur' }
      ]
    };

    // 提交表单
    const submitForm = () => {
      resetFormRef.value.validate((valid) => {
        if (valid) {
          // 这里可以调用重置密码的 API
          ElMessage.success('密码重置成功！');
          console.log('重置密码信息:', resetForm.value);
        } else {
          ElMessage.error('请检查表单填写是否正确');
          return false;
        }
      });
    };

    // 重置表单
    const resetFormFields = () => {
      resetFormRef.value.resetFields();
    };

    return {
      resetFormRef,
      resetForm,
      rules,
      submitForm,
      resetFormFields
    };
  }
};
</script>

<style scoped>
.reset-password-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f5f5f5;
}

.reset-password-card {
  width: 500px;
  padding: 20px;
}

h2 {
  color:#673ab7;
  text-align: center;
  margin-bottom: 20px;
}
</style>