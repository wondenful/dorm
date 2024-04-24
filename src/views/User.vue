<template>
  <div>
    <div style="padding: 10px">
      <el-input
        style="width: 250px"
        suffix-icon="el-icon-search"
        placeholder="请输入名称搜索"
        v-model="userName"
      ></el-input>
      <el-input
        style="width: 250px"
        suffix-icon="el-icon-email"
        placeholder="请输入邮箱搜索"
        v-model="email"
      ></el-input>
      <el-input
        style="width: 250px"
        suffix-icon="el-icon-position"
        placeholder="请输入地址搜索"
        v-model="address"
      ></el-input>
      <el-button style="margin-left: 5px" type="primary" @click="load"
        >搜索</el-button
      >
    </div>
    <div style="margin: 10px">
      <el-button type="primary" @click="handleAdd"
        >新增<i class="el-icon-circle-plus"></i
      ></el-button>
      <el-popconfirm
        confirm-button-text="确定"
        cancel-button-text="取消"
        icon="el-icon-info"
        icon-color="red"
        title="您确定删除吗？"
        @confirm="handleDelBatch"
      >
        <el-button
          type="danger"
          slot="reference"
          style="margin-right: 10px; margin-left: 10px"
          >批量删除<i class="el-icon-remove"></i
        ></el-button>
      </el-popconfirm>
      <el-button type="primary">导入<i class="el-icon-bottom"></i></el-button>
      <el-button type="primary">导出<i class="el-icon-top"></i></el-button>
    </div>

    <el-table :data="tableData" @selection-change="handleSelectionChange">
      <el-table-column type="selection" width="55"> </el-table-column>
      <el-table-column prop="id" label="ID" width="80"> </el-table-column>
      <el-table-column prop="username" label="姓名" width="120">
      </el-table-column>
      <el-table-column prop="phone" label="电话" width="160"> </el-table-column>
      <el-table-column prop="email" label="邮箱" width="210"> </el-table-column>
      <el-table-column prop="address" label="地址" width="300">
      </el-table-column>
      <el-table-column prop="nickname" label="昵称" width="120">
      </el-table-column>
      <el-table-column fixed="right" label="操作">
        <template slot-scope="scope">
          <el-button
            type="success"
            size="small"
            icon="el-icon-edit"
            @click="handleEdit(scope.row)"
            >编辑</el-button
          >
          <el-popconfirm
            confirm-button-text="确定"
            cancel-button-text="取消"
            icon="el-icon-info"
            icon-color="red"
            title="您确定删除吗？"
            @confirm="handleDelete(scope.row.id)"
          >
            <el-button
              type="danger"
              size="small"
              slot="reference"
              icon="el-icon-delete"
              style="margin-left: 10px"
              >删除</el-button
            >
          </el-popconfirm>
        </template>
      </el-table-column>
    </el-table>
    <div style="padding: 10px; text-align: left">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="pageNum"
        :page-sizes="[5, 10, 15, 20]"
        :page-size="pageSize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
      >
      </el-pagination>
    </div>
    <el-dialog title="用户信息" :visible.sync="dialogFormVisible" width="30%">
      <el-form :model="form" label-width="80px" size="small">
        <el-form-item label="用户名">
          <el-input v-model="form.username" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="昵称">
          <el-input v-model="form.nickname" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="邮箱">
          <el-input v-model="form.email" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="电话">
          <el-input v-model="form.phone" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="地址">
          <el-input v-model="form.address" autocomplete="off"></el-input>
        </el-form-item>
        <!-- <el-form-item label="活动区域" :label-width="formLabelWidth">
                <el-select v-model="form.region" placeholder="请选择活动区域">
                  <el-option label="区域一" value="shanghai"></el-option>
                  <el-option label="区域二" value="beijing"></el-option>
                </el-select>
              </el-form-item> -->
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="insert">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: "UserM",
  data() {
    return {
      tableData: [],
      pageNum: 1,
      pageSize: 5,
      total: 0,
      userName: "",
      email: "",
      address: "",
      dialogFormVisible: false,
      form: {},
      multipleSelection: [],
    };
  },
  created() {
    this.load();
  },
  methods: {
    load() {
      // 请求分页查询数据;
      // fetch(
      //   "http://localhost:8084/user/page?pageNum=" +
      //     this.pageNum +
      //     "&pageSize=" +
      //     this.pageSize +
      //     "&userName=" +
      //     this.userName
      // )
      //   .then((res) => res.json())
      //   .then((res) => {

      // 使用axios方式封装的request分页查询数据
      this.request
        .get("http://localhost:8084/user/page", {
          params: {
            pageNum: this.pageNum,
            pageSize: this.pageSize,
            userName: this.userName,
            email: this.email,
            address: this.address,
          },
        })
        .then((res) => {
          console.log(res);
          this.tableData = res.records;
          this.total = res.total;
        });
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
      this.pageSize = val;
      this.load();
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
      this.pageNum = val;
      this.load();
    },
    handleAdd() {
      this.dialogFormVisible = true;
      this.form = {};
    },
    insert() {
      this.request.post("http://localhost:8084/user", this.form).then((res) => {
        if (res) {
          this.$message.success("保存成功");
          this.dialogFormVisible = false;
          this.load();
        } else {
          this.$message.error("保存失败");
        }
      });
    },
    handleEdit(row) {
      this.form = row;
      this.dialogFormVisible = true;
    },
    handleDelete(id) {
      this.request.delete("http://localhost:8084/user/" + id).then((res) => {
        if (res) {
          this.$message.success("删除成功");
          this.dialogFormVisible = false;
          this.load();
        } else {
          this.$message.error("删除失败");
        }
      });
    },
    handleSelectionChange(val) {
      this.multipleSelection = val;
    },
    handleDelBatch() {
      let ids = this.multipleSelection.map((v) => v.id);
      this.request
        .post("http://localhost:8084/user/del/batch", ids)
        .then((res) => {
          if (res) {
            this.$message.success("批量删除成功");
            this.dialogFormVisible = false;
            this.load();
          } else {
            this.$message.error("批量删除失败");
          }
        });
    },
  },
};
</script>
