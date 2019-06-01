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
export default {
    name: 'userinfo',
    data () {
        return {
            msg: 'false'
        }
    },
    computed: {
        logined () {
            return this.$store.state.logined;
        }
    },
    methods: {
        logout () {
            this.$store.commit('logout');
        }
    }
}
</script>

<style lang="scss" scoped>

</style>
