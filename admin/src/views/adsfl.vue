<template>
    <div id="editfl">
        <h1>{{id?"编辑":"新建"}}广告位</h1>
        <el-form label-width="120px" @submit.native.prevent="save">

            <el-form-item label="名称">
            <el-input v-model="model.name"></el-input>
            </el-form-item>
            <el-form-item label="广告">
                            <el-button size="small" @click="model.items.push({})"> <i class="el-icon-plus"></i>添加幻灯片</el-button>
            <el-row type="flex" style="flex-wrap:wrap">
                <el-col :md="24" v-for="(item,index) in model.items" :key="index">
                   <el-form-item label="跳转链接(url)">
                       <el-input v-model="item.url"></el-input>
                    </el-form-item>
                    <el-form-item label="图片" style="margin-top:0.5rem">
                    <el-upload
                        class="avatar-uploader"
                        :action="$http.defaults.baseURL + '/upload'"
                        :show-file-list="false"
                        :on-success="res => $set(item,'image',res.imgUrl)">
                        <img v-if="item.image" :src="item.image" class="avatar">
                        <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                    </el-upload>
                    </el-form-item>
                    <el-form-item>
                       <el-button size="small" type="danger" @click="model.items.splice(index,1)">删除</el-button>
                    </el-form-item>                
                </el-col>
            </el-row>
            </el-form-item>

            <el-form-item>
            <el-button type="primary" native-type="submit">保存</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>
<script>
export default {
    props:['id'],
    data(){
        return{
            model:{
                items:[]
            },
        }
    },
    methods:{
        async save(){
            let res
            if(this.id){
                res = await this.$http.put(`restful/ads/${this.id}`,this.model)
            }else{
                res = await this.$http.post('restful/ads',this.model)
            }
            console.log(res)
            this.$message({
                type:'success',
                message:'保存成功'
            })
            this.$router.push('/ads/lb')
        },

        async fetch(){
            let res = await this.$http.get(`restful/ads/${this.id}`)
            this.model = Object.assign({},this.model,res.data)
        }
    },
    mounted(){

        this.id && this.fetch()
    }
}
</script>