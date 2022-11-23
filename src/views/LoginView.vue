<template>
  <div class="login-box">
    <el-form
      ref="ruleFormRef"
      :model="ruleForm"
      status-icon
      :rules="rules"
      label-width="120px"
      class="demo-ruleForm"
    >
      <h2>后台管理系统</h2>
      <el-form-item label="账号" prop="username">
        <el-input v-model="ruleForm.username" autocomplete="off" />
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input
          v-model="ruleForm.password"
          type="password"
          autocomplete="off"
        />
      </el-form-item>
      <el-form-item>
        <el-button
          type="primary"
          class="loginBtn"
          @click="submitForm(ruleFormRef)"
          >登录</el-button
        >
        <el-button class="loginBtn" @click="resetForm()"
          >重置</el-button
        >
      </el-form-item>
    </el-form>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs, ref } from "vue";
import { LoginData } from "../type/login";
import { useRouter } from "vue-router";
import type { FormInstance } from "element-plus";
import {login} from '../request/api'
export default defineComponent({
  setup() {
    const data = reactive(new LoginData());
    const rules = {
      username: [
        {
          required: true,
          message: "请输入账号",
          trigger: "blur",
        },
        {
          min: 3,
          max: 10,
          message: "账号长度在3-10之间",
          trigger: "blur",
        },
      ],
      password: [
        {
          required: true,
          message: "请输入密码",
          trigger: "blur",
        },
        {
          min: 3,
          max: 10,
          message: "密码长度在3-10之间",
          trigger: "blur",
        },
      ],
    };
    // 登录
    const ruleFormRef = ref<FormInstance>();
    const router=useRouter()
    const submitForm = (formEl: FormInstance | undefined) => {
      if (!formEl) return;
      formEl.validate((valid) => {
        if (valid) {
          login(data.ruleForm).then((res)=>{
            // 将token进行保存
            localStorage.setItem("token", res.data.token)
            // 跳转页面,首页
            router.push('/')
            
          })
        } else {
          console.log("error submit!");
          return false;
        }
      });
    };

    //重置
    const resetForm = () => {
      (data.ruleForm.username = ""), (data.ruleForm.password = "");
    };
    return { ...toRefs(data), rules, resetForm ,submitForm,ruleFormRef};
  },
});
</script>

<style lang="scss" scoped>
.login-box {
  width: 100%;
  height: 700px;
  background: url("../assets/bg.jpg");
  overflow: hidden;
  text-align: center;
  h2 {
    margin-bottom: 15px;
  }
}
.demo-ruleForm {
  width: 500px;
  margin: 200px auto;
}
.loginBtn {
  width: 48%;
}
</style>
