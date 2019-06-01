<template>
    <div>
        {{ logined }}
        <div v-if="logined">
            用户信息
        </div>
        <button @click="logout">退出</button>
    </div>
</template>

<script>
import { mapState } from 'vuex';

export default {
    name: 'userinfo',
    data () {
        return {
            msg: 'false'
        }
    },
    computed: mapState([
        'logined'
    ]),
    methods: {
        logout () {
            localStorage.removeItem('logined')
            localStorage.removeItem('userInfo')
            this.$store.commit('logout')
            this.$router.replace({ path: '/' })
        }
    }
}
</script>

<style lang="scss" scoped>

</style>
