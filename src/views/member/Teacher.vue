<template>
  <div class="member-teacher">
    <el-table :data="tableData" style="width: 100%">
      <el-table-column type="index" width="50" label="序号"></el-table-column>
      <el-table-column prop="name" label="姓名" width="90"></el-table-column>
      <el-table-column prop="teacherId" width="120" label="工号"></el-table-column>
      <el-table-column width="120" label="性别">
        <template slot-scope="scope">
          <span>{{ scope.row.sex == 1 ? "男" : "女" }}</span>
        </template>
      </el-table-column>
      <el-table-column prop="teachYear" label="教龄" width="80">
        <template slot-scope="scope">{{ scope.row.teachYear + "年" }}</template>
      </el-table-column>
      <el-table-column prop="address" label="地址"></el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button size="mini" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
          <el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>

    <el-dialog title="编辑老师" :visible.sync="dialogFormVisible" :modal-append-to-body="false">
      <el-form :model="form" label-width="80" :inline="true">
        <el-form-item label="姓名">
          <el-input v-model="form.name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="性别">
          <el-select v-model="form.sex" placeholder="请选择性别">
            <el-option label="男" value="1"></el-option>
            <el-option label="女" value="0"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="工号">
          <el-input v-model="form.teacherId" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="工龄">
          <el-input v-model="form.teachYear" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="地址">
          <el-input v-model="form.address" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="submitEdit">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
const TABLE_DATA = [
  {
    id: 0,
    name: '王小虎',
    teachYear: '2016-05-02',
    address: '上海市普陀区金沙江路 1518 弄',
    sex: 1,
    teacherId: 'T-10000',
  },
]
export default {
  data() {
    return {
      dialogFormVisible: false,
      curEditIndex: 0,
      form: {
        id: '',
        name: '',
        teachYear: 0,
        address: '',
        sex: '',
        teacherId: 'T-10000',
      },
    }
  },
  computed: {
    tableData() {
      return this.$store.state.teacherList
    },
  },
  methods: {
    handleEdit(index, row) {
      this.form = JSON.parse(JSON.stringify(this.tableData[index]))
      this.form.sex = this.form.sex == 1 ? "男": "女"
      this.curEditIndex = index
      this.dialogFormVisible = true
    },

    submitEdit() {
      this.$store.dispatch('editTeacherFun', {
        index: this.curEditIndex,
        form: this.form,
      })
      this.tableData[this.curEditIndex] = this.form
      this.dialogFormVisible = false
    },
    handleDelete(index, row) {
      console.log(index, row)
      this.$store.dispatch('deleteTeacherFun', { index: index })
    },
  },
  created() {
  },
}
</script>