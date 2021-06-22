<template>
  <div class="login">
    <div class="login_window">
      <h1>{{ $route.name }}</h1>
      <div class="login_info">
        <span class="svg-container">
          <svg-icon icon-class="user" />
        </span>
        <el-input
          placeholder="请输入用户名"
          v-model="form.username"
          id="user"
        ></el-input>
      </div>
      <div class="login_info">
        <span class="svg-container">
          <svg-icon icon-class="password"></svg-icon>
        </span>
        <el-input
          placeholder="请输入密码"
          v-model="form.password"
          show-password
          id="pass"
          @keyup.enter.native="goIndex()"
        ></el-input>
      </div>
      <el-button type="primary" round id="sub" @click="goIndex()"
        >登录</el-button
      >
    </div>
  </div>
</template>
<script>
import { mapGetters, mapActions } from "vuex";
import { successAlert, warningAlert } from "../../utils/alert";
import { login } from "../../utils/request";
export default {
  props: [],
  components: {},
  data() {
    return {
      form: {
        username: "",
        password: "",
      },
    };
  },
  computed: {
    ...mapGetters({
      userInfo: "userInfo",
    }),
  },
  methods: {
    ...mapActions({
      reqUserinfo: "reqUserinfo",
    }),
    //登录
    goIndex() {
      login(this.form).then((res) => {
        if (res.data.code == 200) {
          //1成功的页面
          //2本地储存
          // localStorage.setItem("user", this.userInfo.token);
          //2存入状态层
          this.reqUserinfo(res.data.list);
          successAlert(res.data.msg);
          //3跳入页面
          this.$router.push("/home");
        } else {
          warningAlert(res.data.msg);
        }
      });
    },
  },

  mounted() {},
};
</script>
<style lang="scss" scoped>
$loginWidth: 360px;
$loginHeight: 240px;
$loginColor: #f1f1f1;
$loginRadius: 5px;

@mixin center {
  text-align: center;
}

@mixin flex($display: flex, $marginTop: 16px) {
  display: $display;
  justify-content: space-between;
  margin: $marginTop;
}

@mixin svgicon($iconMarginV: 4px, $iconMarginH:4px) {
  margin: $iconMarginH $iconMarginV;
}
.login {
  position: relative;
  width: 100vw;
  height: 100vh;
  background: linear-gradient(skyblue, blue);
  .login_window {
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    width: $loginWidth;
    height: $loginHeight;
    background-color: $loginColor;
    margin: auto;
    border-radius: $loginRadius;
    padding: 30px 30px;
  }
  .svg-container {
    @include svgicon(8px, 10px)
  }
  h1 {
    @include center;
  }
  .login_info {
    @include flex();
  }
  #sub {
    width: 328px;
    margin-left: 12px;
  }
}
</style>
