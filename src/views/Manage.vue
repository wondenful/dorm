<template>
  <div style="height: 100%" class="home">
    <el-container style="height: 100%; border: 1px solid #eee">
      <el-aside width="auto"> <Aside></Aside> </el-aside>
      <el-container style="height: 100%">
        <el-header
          style="
            text-align: right;
            font-size: 12px;
            border-bottom: 1px solid red;
            line-height: 60px;
          "
          ><Header name="小明"></Header
        ></el-header>
        <el-main> <router-view></router-view></el-main>
      </el-container>
    </el-container>
  </div>
</template>

<!-- <script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "HomeView",
  components: {
    HelloWorld,
  },
};
</script> -->

<script>
import Aside from "@/components/Aside.vue";
import Header from "@/components/Header.vue";

export default {
  name: "ManageM",
  components: {
    Aside,
    Header,
  },
};
</script>

<style scoped>
.el-main {
  text-align: left;
}
</style>

<style>
.el-menu-vertical-demo:not(.el-menu-collapse) {
  width: 200px;
  min-height: 400px;
}
</style>
