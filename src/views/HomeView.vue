<template>
  <div class="common-layout">
    <el-container>
      <el-header height="60px">
        <el-row :gutter="20" height="60px">
          <el-col :span="4"><img src="../assets/logo.jpg" alt="" class="logo"></el-col>
          <el-col :span="16"><h2>后台管理系统</h2></el-col>
          <el-col :span="4"><el-button @click="delToken">退出登录·</el-button></el-col>
        </el-row>
      </el-header>
      <el-container>
        <el-aside width="200px">
          <el-menu
        active-text-color="#ffd04b"
        background-color="#545c64"
        class="el-menu-vertical-demo"
        default-active="active"
        text-color="#fff"
        router
      >
      <!-- router是用来开启路由模式 -->
        <el-menu-item :index="item.path" v-for="item in list" :key="item.path">
          <el-icon><icon-menu /></el-icon>
          <span>{{item.meta.title}}</span>
        </el-menu-item>
      </el-menu>
        </el-aside>
        <el-main>
          <!-- 设置路由出口 -->
          <router-view></router-view>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { useRouter,useRoute } from 'vue-router';
export default defineComponent({
name:'HomeView',
setup(){
const router=useRouter();
const route=useRoute();
const list=router.getRoutes().filter(v=>v.meta.isShow)
// console.log(list);
const delToken=()=>{
localStorage.removeItem('token')
router.push('/login')
}

return {router,list,active:route.path,delToken}
},
components:{},
})
</script>

<style lang="scss" scoped>
.el-header{
  background-color:blanchedalmond
}
.el-col{
  height: 60px;
  text-align: center;
  line-height: 60px;
}
.el-aside .el-menu{
  height:100%;
}
.logo{
  height: 60px;
//  background-color: bisque;
}
h2{
  text-align: center;
  height: 60px;
  line-height: 60px;
  margin-top: 3px;
}
.quit-login{
  text-align: center;
  height: 90px;
  line-height: 60px;
  // background-color: aqua;
}
</style>
