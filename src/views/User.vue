<template>
  <home>
    <div class="user">
      <user-left></user-left>
      <router-view></router-view>
    </div>
  </home>
</template>

<script>
import Home from "./Home";
import UserLeft from "@/components/user/Side/UserLeft";
import UserInfo from "@/components/user/UserInfo";

export default {
  name: "User",
  components: {
    UserInfo,
    UserLeft,
    Home,
  },
};
</script>

<style lang="less" scoped>
.user {
  width: 1200px;
  display: flex;
}
</style>