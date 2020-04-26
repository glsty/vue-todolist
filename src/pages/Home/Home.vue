<template>
  <div class="h-main">
    <el-row>
      <el-col :span="18" :offset="3">
        <div class="h-title-flex">
          <h3>学生名单</h3>
          <el-button 
            @click="handleAdd()"
            plain
            width="120px"
            size="small"
            type="primary"
          >新增</el-button>
        </div>
        <el-table
          ref="multipleTable"
          :data="tableData"
          tooltip-effect="dark"
          style="width: 100%"
          @selection-change="handleSelectionChange"
        >
          <el-table-column 
            prop="name" 
            label="姓名" 
            width="120"
          ></el-table-column>
          <el-table-column 
            prop="sex" 
            label="性别" 
            width="120"
          ></el-table-column>
          <el-table-column 
            prop="address" 
            label="地址" 
            show-overflow-tooltip
          ></el-table-column>
          <el-table-column 
            label="操作" 
            show-overflow-tooltip
          >
          <template slot-scope="scope">
            <el-button 
              @click="handleEdit(scope.$index, scope.row)"
              plain
              class="h-table-button-margin"
            >编辑</el-button>
            <el-popconfirm
              @onConfirm="handleDelete(scope.$index, scope.row)"
              title="这是一段内容确定删除吗？"
            >
              <el-button 
                type="danger" 
                plain
                slot="reference"
              >删除</el-button>
            </el-popconfirm>
          </template>
          </el-table-column>
        </el-table>
      </el-col>
    </el-row>
    <el-dialog :title="title" :visible.sync="dialogFormVisible">
      <el-form :model="form">
        <el-form-item label="学生姓名" :label-width="formLabelWidth">
          <el-input v-model="form.name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="学生住址" :label-width="formLabelWidth">
          <el-input v-model="form.address" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="学生性别" :label-width="formLabelWidth">
          <el-input v-model="form.sex"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="handleEditSubmit">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import { StudentInfo } from '@/utils/info.js'

export default {
  name: "home",
  data() {
    return {
      tableData: StudentInfo,
      currentIndex:0,
      title:'新增学生信息',
      multipleSelection: [],
      dialogFormVisible: false,
      form: {
        name: '',
        address: '',
        sex:''
      },
      formLabelWidth: '120px'
    };
  },
  methods: {
    handleSelectionChange(val) {
      this.multipleSelection = val;
    },
    handleEdit(index) {
      this.dialogFormVisible = true
      this.title = '编辑学生信息'
      this.currentIndex = index
    },
    handleDelete(index) {
      this.tableData.splice(index,1)
    },
    handleEditSubmit(){
        console.log(this.name)
        let {form} = this
        let {name,sex,address} = form
        if(!(name&&sex&&address)){
            this.$message({
            message: '填写内容不能为空',
            type: 'warning'
          });
          return
        }
        if(this.title === '编辑学生信息'){
          this.tableData.splice(this.currentIndex,1,{name,sex,address})
        } else {
          this.tableData.push({name,sex,address})
        }
        this.dialogFormVisible = false
        this.form = {
            name:'',
            sex:'',
            address:''
        }
    },
    handleAdd () {
      this.title = '新增学生信息'
      this.dialogFormVisible = true
    }
  }
};
</script>

<style scoped>
.h-table-button-margin{
  margin-right: 20px
}
.h-title-flex{
  display: flex;
  justify-content: space-between;
}
</style>