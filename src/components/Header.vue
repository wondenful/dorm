<template>
  <el-dropdown>
    <span class="el-dropdown-link">
      {{ user.nickname
      }}<i class="el-icon-arrow-down" style="margin-left: 5px"></i>
    </span>
    <el-dropdown-menu slot="dropdown">
      <el-dropdown-item>个人信息</el-dropdown-item>
      <el-dropdown-item>
        <div @click="logout">
          <span style="text-decoration: none"> 退出 </span>
        </div>
      </el-dropdown-item>
    </el-dropdown-menu>
  </el-dropdown>
</template>

<script>
export default {
  name: "HeaderM",
  props: {
    name: String,
  },
  data() {
    return {
      user: localStorage.getItem("user")
        ? JSON.parse(localStorage.getItem("user"))
        : {},
    };
  },
  methods: {
    logout() {
      this.$router.push("/login");
      localStorage.removeItem("user");
      this.$message.success("退出成功");
    },
  },
};
</script>
