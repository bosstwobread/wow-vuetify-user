<template>
  <v-card-text class="mx-auto">
    <v-form ref="form" v-model="valid" algin="center" lazy-validation>
      <v-text-field v-model="name" :counter="32" :rules="[nameRules]" label="用户名" required></v-text-field>
      <v-text-field type="password" v-model="password" :rules="pwdRules" label="密码" required></v-text-field>
      <v-text-field
        type="password"
        v-model="confirmPassword"
        :rules="[pwd_verfifyRule]"
        label="再次输入密码"
        required
      ></v-text-field>
      <v-btn color="error" class="mr-4" @click="register">注册</v-btn>
    </v-form>
  </v-card-text>
</template>
<style>
</style>
<script>
import axios from "axios";
// import { methods } from "../JS/Register";
export default {
  data: () => ({
    valid: true,
    name: "",
    password: "",
    confirmPassword: "",
    pwdRules: [v => !!v || "请输入密码"]
  }),
  methods: {
    nameRules(v) {
      if (v.length == 0) {
        return "用户名不能为空";
      }
      if (v.length > 32) {
        return "用户名长度不能超过32个";
      }
      if (iChina(v)) {
        return "用户名不能是中文";
      }
      return true;
      function iChina(str) {
        if (/.*[\u4e00-\u9fa5]+.*/.test(str)) {
          return true;
        } else {
          return false;
        }
      }
    },
    pwd_verfifyRule(value) {
      return value === this.password || "密码输入不一致";
    },
    register() {
      if (this.$refs.form.validate()) {
        //先执行验证
        axios
          .post("http://bosstwobread.com:8010/register/addUser", {
            name: this.name,
            pwd: this.password
          })
          .then(function(response) {
            if (response.data.iSuccess === false) {
              console.log(response.data);
              alert("账号已存在");
              return false;
            } else {
              alert("注册成功");
            }
          })
          .catch(function(error) {
            console.log(error);
          });
      }
    }
  }
};
</script>