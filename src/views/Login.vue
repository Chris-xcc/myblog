<template>
  <home>
    <div class="login">
      <div class="container">
        <div class="top">登录</div>
        <el-form :model="loginForm" ref="loginForm" status-icon>
          <el-form-item prop="username">
            <el-input
              type="text"
              v-model="loginForm.username"
              autocomplete="on"
              prefix-icon="el-icon-s-custom"
              placeholder="邮箱"
            ></el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input
              type="password"
              v-model="loginForm.password"
              autocomplete="off"
              prefix-icon="el-icon-lock"
              placeholder="密码"
            ></el-input>
          </el-form-item>
          <!-- <el-form-item>
              <el-checkbox v-model="loginForm.remember" @click="getRemember">记住密码</el-checkbox>
          </el-form-item>-->

          <el-form-item>
            <el-button type="primary" @click="login()">登录</el-button>
          </el-form-item>
          <el-form-item>
            <el-button @click="resetForm('loginForm')">重置</el-button>
          </el-form-item>
        </el-form>
        <div class="bottom-box">
          <div class="right">
            <!-- <router-link to="email_active" tag="div">
              <div class="active">激活</div>
            </router-link>
            <span>|</span>-->
            <router-link to="register" tag="div">
              <div class="register">注册</div>
            </router-link>
            <span>|</span>
            <router-link to="forget" tag="div">
              <div class="forget">忘记密码</div>
            </router-link>
          </div>
        </div>
      </div>
    </div>
  </home>
</template>


<script>
import Home from "./Home";
import { SET_TOKEN } from "@/store/mutations-types";
import { post } from "@/api/services/instance";
import { get } from "../api/services/instance";
import { GET_USERNAME } from "../store/mutations-types";

export default {
  name: "Login",
  components: {
    Home,
  },
  data() {
    return {
      loginForm: {
        username: "",
        password: "",
        // remember: false,
      },
    };
  },
  methods: {
    getRemember() {
      this.remember = !this.remember;
    },
    getCookie(name) {
      let value = "; " + document.cookie;
      let parts = value.split("; " + name + "=");
      if (parts.length === 2) return parts.pop().split(";").shift();
    },
    login() {
      post({
        url: "/login/",
        data: this.loginForm,
        headers: { "X-CSRFToken": this.getCookie("csrftoken") },
      })
        .then((response) => {
          // this.Authorization = response.data.token;
          // console.log(response);
          window.localStorage.setItem("token", "Token " + response.data.token);
          this.$store.commit({
            type: SET_TOKEN,
            token: "Token " + response.data.token,
          });
          this.$router.push("/");
          alert("登陆成功");

          get({ url: "/users/1/" })
            .then((response) => {
              window.localStorage.setItem("username", response.data.username);
              this.$store.commit({
                type: GET_USERNAME,
                username: response.data.username,
              });
            })
            .catch((err) => {
              console.log(err);
            });
        })
        .catch((err) => {
          console.log(err);
        });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
      // console.log('reset');
    },
  },
};
</script>


<style lang="less" scoped>
@import url("@/assets/css/login/login.less");
</style>