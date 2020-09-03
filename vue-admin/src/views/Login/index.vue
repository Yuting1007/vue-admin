<template>
  <div id="login">
    <div class="login-wrap">
      <ul class="menu-tab">
        <li
          v-for="item in menuTab"
          :key="item.id"
          :class="{ current: item.current }"
          @click="toggleMenu(item)"
        >
          {{ item.txt }}
        </li>
      </ul>
      <!-- login form -->
      <el-form
        :model="ruleForm"
        status-icon
        :rules="rules"
        ref="ruleForm"
        class="loginForm"
        size="medium"
      >
        <el-form-item prop="email" class="itemForm">
          <label>Email</label>
          <el-input
            type="text"
            v-model="ruleForm.email"
            autocomplete="off"
          ></el-input>
        </el-form-item>
        <el-form-item prop="password" class="itemForm">
          <label>Password</label>
          <el-input
            type="password"
            v-model="ruleForm.password"
            autocomplete="off"
            minlength="6"
            maxlength="20"
          ></el-input>
        </el-form-item>
        <el-form-item prop="code" class="itemForm">
          <label>Validation Code</label>
          <el-row :gutter="11">
            <el-col :span="16"
              ><el-input
                v-model.number="ruleForm.code"
                minlength="6"
                maxlength="6"
              ></el-input
            ></el-col>
            <el-col :span="8"
              ><el-button type="success" class="block"
                >Get Code</el-button
              ></el-col
            >
          </el-row>
        </el-form-item>
        <el-form-item>
          <el-button
            type="danger"
            class="login_btn block"
            @click="submitForm('ruleForm')"
            >Submit</el-button
          >
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
import { validate_email, validate_password } from "@/utils/validate";
export default {
  name: "Login",
  data() {
    //validate email
    var validateEmail = (rule, value, callback) => {
      let regEmail = validate_email(value);
      if (value === "") {
        callback(new Error("Please input the email"));
      } else if (!regEmail) {
        callback(new Error("Please input the email in right format"));
      } else {
        callback();
      }
    };
    //validate password
    var validatePassword = (rule, value, callback) => {
      let regPassword = validate_password(value);
      if (value === "") {
        callback(new Error("Please input the password"));
      } else if (!regPassword) {
        callback(
          new Error(
            "The length must be larger than 5 and smaller than 20, including numbers and letters"
          )
        );
      } else {
        callback();
      }
    };
    //validate code
    const validateCode = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("Please input the code"));
      } else if (value.length !== 6) {
        callback(new Error("The code must be 6 digits long"));
      } else {
        callback();
      }
    };
    return {
      menuTab: [
        { txt: "Login", current: true },
        { txt: "Sign Up", current: false }
      ],
      isActive: true,
      //form data
      ruleForm: {
        email: "",
        password: "",
        code: ""
      },
      rules: {
        email: [{ validator: validateEmail, trigger: "blur" }],
        password: [{ validator: validatePassword, trigger: "blur" }],
        code: [{ validator: validateCode, trigger: "blur" }]
      }
    };
  },
  create() {},
  mounted() {},
  methods: {
    toggleMenu(data) {
      this.menuTab.forEach(elem => {
        elem.current = false;
      });
      data.current = true;
    },
    //form methods
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          alert("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    }
  }
};
</script>
<style lang="scss" scoped>
#login {
  height: 100vh;
  background-color: #344a5f;
}
.login-wrap {
  width: 330px;
  margin: auto;
}
.menu-tab {
  text-align: center;
}
li {
  display: inline-block;
  width: 88px;
  line-height: 36px;
  font-size: 14px;
  color: #fff;
  border-radius: 2px;
  cursor: pointer;
}
.current {
  background-color: rgba(0, 0, 0, 0.1);
}
.loginForm {
  margin-top: 29px;
  label {
    display: block;
    margin-bottom: 3px;
    font-size: 14px;
    color: #fff;
  }
  .itemForm {
    margin-bottom: 13px;
  }
  .block {
    display: block;
    width: 100%;
  }
  .login_btn {
    margin-top: 19px;
  }
}
</style>
