<template>
  <div>
    用户信息
    <button @click="logout">退出</button>
  </div>
</template>

<script>
export default {
  name: 'userinfo',
  data(){
    return {
      logined: false
    }
  },
  methods: {
    logout(){
      this.$store.commit('logout');
    }
  }
}
</script>

<style lang="scss" scoped>

</style>
