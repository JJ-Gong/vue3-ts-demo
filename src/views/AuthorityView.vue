<template>
    <div>
        <el-tree
        ref="treeRef"
            :data="list"
            show-checkbox
            node-key="id"
           
            :default-expanded-keys="[2,3]"
            :default-checked-keys="authority"
            :props="{
                children:'roleList',
                label:'name',
            }"
        />
        <el-button type="primary" @click="changeAuthority">
          确认修改
        </el-button>
    </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, reactive, toRefs } from 'vue'
import { useRoute } from 'vue-router'
import {InitData} from '../type/authority'
import {getAuthorityList} from '../request/api'
export default defineComponent({
    setup () {
        const route=useRoute()
        const params:any=route.params
        const data=reactive(new InitData(params.id,params.authority))
        onMounted(()=>{
            getAuthorityList().then(res=>{
                console.log(res);
                data.list=res.data
            })
        })
        const changeAuthority=()=>{

        }
        return {...toRefs(data),changeAuthority}
    }
})
</script>

<style scoped>

</style>