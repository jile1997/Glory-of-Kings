<template>
    <div id="catlb">
        <h1>英雄列表</h1>
        <el-table :data="items">
            <el-table-column prop="_id" label="ID" width="220">
            </el-table-column>

            <el-table-column prop="name" label="英雄名称">
            </el-table-column>
            <el-table-column prop="title" label="称号">
            </el-table-column>
            <el-table-column prop="avatar" label="头像">
              <template slot-scope="scope">
                <img :src="scope.row.avatar" alt="" style="height:3rem">

              </template>
            </el-table-column>
            <el-table-column
            fixed="right"
            label="操作"
            width="180">
            <template slot-scope="scope">
                <el-button type="text" size="small" @click="$router.push(`/hero/fl/${scope.row._id}`)">编辑</el-button>
                <el-button type="text" size="small" @click="remove(scope.row)">删除</el-button>
            </template>
            </el-table-column>
      </el-table>
    </div>
</template>
<script>
export default {
    data(){
        return{
            items:[]
        }
    },
    methods:{
        async getitems(){
            let res = await this.$http.get('restful/heros')
            this.items = res.data
        },
        async remove(row){
        // console.log(row)
        this.$confirm(`是否要删除分类 "${row.name}"`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(async() => {
          let res = await this.$http.delete(`restful/heros/${row._id}`)
          this.$message({
            type: 'success',
            message: res.data.msg
          });
          this.getitems()
        })
      }
    },
    created(){
        this.getitems()
    }
}
</script>
