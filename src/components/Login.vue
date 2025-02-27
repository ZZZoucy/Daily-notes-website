<template>
  <div id="login">
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
          <div class="main"></div>
          <div class="form">
            <h3 @click="showRegister">创建账户</h3>
            <!-- 切换创建账户和登录的样式 -->
            <transition name="slide">
              <div v-bind:class="{ show: isShowRegister }" class="register">
                <input
                  type="text"
                  v-model="register.username"
                  placeholder="用户名"
                />
                <input
                  type="password"
                  v-model="register.password"
                  @keyup.enter="onRegister"
                  placeholder="密码"
                />
                <p v-bind:class="{ error: register.isError }">
                  {{ register.notice }}
                </p>
                <div class="button" @click="onRegister">创建账号</div>
              </div>
            </transition>
            <h3 @click="showLogin">登录</h3>
            <!-- 切换创建账户和登录的样式 -->
            <transition name="slide">
              <div v-bind:class="{ show: isShowLogin }" class="login">
                <input
                  type="text"
                  v-model="login.username"
                  placeholder="输入用户名"
                />
                <input
                  type="password"
                  v-model="login.password"
                  @keyup.enter="onLogin"
                  placeholder="密码"
                />
                <p v-bind:class="{ error: login.isError }">
                  {{ login.notice }}
                </p>
                <div class="button" @click="onLogin">登录</div>
              </div>
            </transition>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from "vuex";

export default {
  data() {
    return {
      isShowLogin: true,
      isShowRegister: false,
      // notice 表示出错是展示的提示信息。
      // isError 即表示是否出错，为 true 的时候展示 notice。
      login: {
        username: "",
        password: "",
        notice: "输入用户名和密码",
        isError: false
      },
      register: {
        username: "",
        password: "",
        notice: "创建账号后，请记住用户名和密码",
        isError: false
      }
    };
  },
  methods: {
    ...mapActions({
      loginUser: "login",
      registerUser: "register"
    }),

    showLogin() {
      this.isShowLogin = true;
      this.isShowRegister = false;
    },
    showRegister() {
      this.isShowLogin = false;
      this.isShowRegister = true;
    },
    onRegister() {
      if (!/^[\w\u4e00-\u9fa5]{3,15}$/.test(this.register.username)) {
        this.register.isError = true;
        this.register.notice = "用户名3~15个字符，仅限于字母数字下划线中文";
        return;
      }
      if (!/^.{6,16}$/.test(this.register.password)) {
        this.register.isError = true;
        this.register.notice = "密码长度为6~16个字符";
        return;
      }
      // 注册账号成功跳转至 notebooks
      this.registerUser({
        username: this.register.username,
        password: this.register.password
      })
        .then(() => {
          // 如果没有错误，则不显示任何错误提示
          this.register.isError = false;
          this.register.notice = "";
          // 如果注册成功了，就跳转到笔记本列表页
          this.$router.push({ path: "notebooks" });
        })
        .catch(data => {
          this.register.isError = true;
          this.register.notice = data.msg;
        });
    },
    onLogin() {
      if (!/^[\w\u4e00-\u9fa5]{3,15}$/.test(this.login.username)) {
        this.login.isError = true;
        this.login.notice = "用户名3~15个字符，仅限于字母数字下划线中文";
        return;
      }
      if (!/^.{6,16}$/.test(this.login.password)) {
        this.login.isError = true;
        this.login.notice = "密码长度为6~16个字符";
        return;
      }
      // 登录成功跳转至 notebooks
      this.loginUser({
        username: this.login.username,
        password: this.login.password
      })
        .then(() => {
          this.login.isError = false;
          this.login.notice = "";
          this.$router.push({ path: "notebooks" });
        })
        .catch(data => {
          this.login.isError = true;
          this.login.notice = data.msg;
        });
    }
  }
};
</script>

<style lang="less" scoped>
.modal-mask {
  position: fixed;
  z-index: 100;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  display: table;
  transition: opacity 0.3s ease;
}
.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}
.modal-container {
  max-width: 800px;
  height: 500px;
  margin: 0px auto;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
  display: flex;
  .main {
    flex: 1;
    background: #36bc64 url(../assets/images/page.jpeg) center center no-repeat;
    background-size: contain;
  }
  .form {
    width: 270px;
    border-left: 1px solid #ccc;
    overflow: hidden;
    h3 {
      padding: 10px 20px;
      margin-top: -1px;
      font-weight: normal;
      font-size: 16px;
      border-top: 1px solid #eee;
      cursor: pointer;
      &:nth-of-type(2) {
        border-bottom: 1px solid #eee;
      }
    }
    .button {
      background-color: #2bb964;
      height: 36px;
      line-height: 36px;
      text-align: center;
      font-weight: bold;
      color: #fff;
      border-radius: 4px;
      margin-top: 18px;
      cursor: pointer;
    }
    .login,
    .register {
      padding: 0px 20px;
      border-top: 1px solid #eee;
      height: 0;
      overflow: hidden;
      /* 创建账户和登录的动画是 height 0～193 的变化 */
      transition: height 0.4s;
      &.show {
        height: 193px;
      }
      input {
        display: block;
        width: 100%;
        height: 35px;
        line-height: 35px;
        padding: 0 6px;
        border-radius: 4px;
        border: 1px solid #ccc;
        outline: none;
        font-size: 14px;
        margin-top: 10px;
      }
      input:focus {
        border: 3px solid #9dcaf8;
      }
      p {
        font-size: 12px;
        margin-top: 10px;
        color: #444;
      }
      .error {
        color: red;
      }
    }
    .login {
      border-top: 0;
    }
  }
}
</style>
