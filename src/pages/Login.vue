<template>
  <div class="main">
    <div class="title">
      <h3>登录</h3>
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
          <button @click="login">登录</button>
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
import axios from 'axios';
import {config} from '../assets/scripts/config.js';

export default {
  name: 'login',
  data () {
    return {
      msg: '登录页',
      form: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    login () {
      let _this = this;

      axios.get('http://' + config.host + ':'+ config.port +'/users?username=' + this.form.username)
        .then(function(response){
          console.log(response.data[0]);
          if(response.data[0].password === this.form.password){
            console.log("OK");
            var userInfo = {
              id: response.data[0].id,
              username: response.data[0].username,
              word: response.data[0].word,
              portrait: response.data[0].portrait,
              constellation: response.data[0].constellation,
              email: response.data[0].email
            };
            localStorage.userInfo = JSON.stringify(userInfo);
            this.$router.push({ path: '/' });
          }

          _this.$store.commit('login');
        }.bind(this))
        .catch(function(error){
          console.log(error);
        });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
  @import './../assets/styles/entry.scss';
</style>
