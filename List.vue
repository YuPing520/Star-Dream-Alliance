<template>
  <div>

    <el-form :inline="true" :model="stu" class="demo-form-inline">

      <el-form-item label="学生名称">
        <el-input v-model="stu.stuName" placeholder="请输入要模糊查询的学生名称"></el-input>
      </el-form-item>

      <el-form-item label="日期">
        <el-date-picker type="datetime" value-format="yyyy-MM-dd HH:mm:ss" placeholder="请选择开始日期" v-model="stu.start" style="width: 100%;"></el-date-picker>
        -----
        <el-date-picker type="datetime" value-format="yyyy-MM-dd HH:mm:ss" placeholder="请选择结束日期" v-model="stu.end" style="width: 100%;"></el-date-picker>
      </el-form-item>


      <el-form-item label="专业名称">
        <el-select v-model="stu.stuMajorId" placeholder="请选择专业">
          <el-option v-for="i in major" :key="i.majorId" :value="i.majorId" :label="i.majorName"></el-option>
        </el-select>
      </el-form-item>


      <el-form-item>
        <el-button type="primary" @click="onSubmit">查询</el-button>
      </el-form-item>

    </el-form>









    <h1 style="align-content: center;color: fuchsia">学生表</h1>

    <el-table :data="arr" style="width: 100%" @selection-change="handleSelectionChange">

      <el-table-column
        type="selection"
        width="55">
      </el-table-column>

      <el-table-column label="编号" width="180">
        <template slot-scope="scope">
          <span style="margin-left: 10px">{{ scope.row.stuId }}</span>
        </template>
      </el-table-column>


      <el-table-column label="名称" width="180">
        <template slot-scope="scope">
          <span style="margin-left: 10px">{{ scope.row.stuName }}</span>
        </template>
      </el-table-column>


      <el-table-column label="年龄" width="180">
        <template slot-scope="scope">
          <span style="margin-left: 10px">{{ scope.row.stuAge }}</span>
        </template>
      </el-table-column>

      <el-table-column label="性别" width="180">
        <template slot-scope="scope">
          <span style="margin-left: 10px">{{ scope.row.stuSex }}</span>
        </template>
      </el-table-column>

      <el-table-column label="日期" width="180">
        <template slot-scope="scope">
          <span style="margin-left: 10px">{{ scope.row.stuTime }}</span>
        </template>
      </el-table-column>

      <el-table-column label="手机号" width="180">
        <template slot-scope="scope">
          <span style="margin-left: 10px">{{ scope.row.stuPhone }}</span>
        </template>
      </el-table-column>

      <el-table-column label="爱好" width="180">
        <template slot-scope="scope">
          <span style="margin-left: 10px">{{ scope.row.stuHobby }}</span>
        </template>
      </el-table-column>

      <el-table-column label="地址" width="180">
        <template slot-scope="scope">
          <span style="margin-left: 10px">{{ scope.row.stuAddress }}</span>
        </template>
      </el-table-column>

      <el-table-column label="专业名称" width="180">
        <template slot-scope="scope">
          <span style="margin-left: 10px">{{ scope.row.stuMajorName }}</span>
        </template>
      </el-table-column>


      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button
            size="mini"
            @click="handleEdit(scope.row)">修改</el-button>
          <el-button
            size="mini"
            type="danger"
            @click="handleDelete(scope.row.stuId)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>

    <el-button type="danger" icon="el-icon-delete" circle @click="deleteAll">批量删除</el-button>

    <router-link tag="button" to="/insert">添加</router-link>

</div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）,
//例如：import 《组件名称》 from '《组件路径》,
import axios from "axios";
  export default {
    //import引入的组件需要注入到对象中才能使用"
    components: {
axios
},
    props: {},
    data() {
      //这里存放数据"
      return {
        stu:{},
        arr:[],
        major:[],
        ids:[]
};
    },
    //计算属性 类似于data概念",
    computed: {},
    //监控data中的数据变化",
    watch: {},
    //方法集合",
    methods: {

      handleSelectionChange(val) {
        this.ids = []
        for (let valKey in val) {
          this.ids.push(val[valKey].stuId)
        }
      },


      deleteAll(){
        if (this.ids.length > 0){
          axios.post('/api/stu/delete',this.ids).then(
            res=>{
              alert(res.data.msg)
              if (res.data.code == 200){
                this.ids = []
                this.queryStu()
              }
            }
          )
        }else {
          alert("至少选择一个数据进行删除");
        }
      },



      handleDelete(stuId){
        this.ids = []
        this.ids.push(stuId)
        axios.post('/api/stu/delete',this.ids).then(
          res=>{
            alert(res.data.msg)
            if (res.data.code == 200){
              this.ids = []
              this.queryStu()
            }
          }
        )
      },


      handleEdit(stu){
        if (stu.stuHobby.length > 0){
          stu.hobby = stu.stuHobby.split(',')
        }else {
          stu.hobby = []
        }

        this.$router.push({path:'/update',query:{stu:stu}})
      },

      onSubmit(){
        this.queryStu()
      },


      queryMajor(){
        axios.get('/api/stu/queryMajor').then(
          res=>{
            this.major = res.data.data
          }
        )
      },

      queryStu(){
        axios.post('/api/stu/queryStu',this.stu).then(
          res=>{
            if (res.data.code == 200){
              this.arr = res.data.data
            }
          }
        )
      }

},
    //生命周期 - 创建完成（可以访问当前this实例）",
    created() {
      this.queryStu()
      this.queryMajor()
    },
    //生命周期 - 挂载完成（可以访问DOM元素）",
    mounted() {
    },
    beforeCreate() {
    }, //生命周期 - 创建之前",
    beforeMount() {
    }, //生命周期 - 挂载之前",
    beforeUpdate() {
    }, //生命周期 - 更新之前",
    updated() {
    }, //生命周期 - 更新之后",
    beforeDestroy() {
    }, //生命周期 - 销毁之前",
    destroyed() {
    }, //生命周期 - 销毁完成",
    activated() {
    } //如果页面有keep-alive缓存功能，这个函数会触发",
  };
</script>
<style scoped>

</style>
