<template>
  <div class="main">
    <div class="title">
      <h3>注册</h3>
    </div>
    <div class="entry-label">

      <form action="#">
        <div>
          <label for="user-name">
            <i class="fa fa-user"></i>
          </label>
          <input type="text" v-model="form.username">
        </div>
        <div>
          <label for="password">
            <i class="fa fa-lock"></i>
          </label>
          <input type="password" v-model="form.password">
        </div>
        <div>
          <button @click="register">注册</button>
        </div>
      </form>
    </div>
    <div>
      <p>
        <router-link to="/register">新用户注册</router-link>
      </p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "register",
  data() {
    return {
      msg: "注册页",
      form: {
        username: "",
        password: ""
      }
    };
  },
  methods: {
    register() {
      var newUser = {
        username: this.form.username,
        password: this.form.password
      };

      axios
        .post("http://localhost:3000/users", newUser)
        .then(function(response) {
          console.log(response);
        })
        .catch(function(error) {
          console.log(error);
        });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
@import "./../assets/styles/entry.scss";
</style>
