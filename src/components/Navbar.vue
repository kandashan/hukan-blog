<template>
    <div class="navbar">
        <router-link class="navbar-nav" to="/" exact>
            <i class="fa fa-fw fa-home"></i>
        </router-link>
        <router-link class="navbar-nav" to="/articleTypes">
            <i class="fa fa-fw fa-compass"></i>
        </router-link>
        <router-link class="navbar-nav" to="/photos">
            <i class="fa fa-fw fa-bell-o"></i>
        </router-link>
        <router-link class="navbar-nav" to="/mine">
            <i class="fa fa-fw fa-user-o"></i>
        </router-link>
    </div>
</template>

<script>
export default {
    name: 'navbar',
    data () {
        return {
            msg: 'navbar'
        }
    },
    methods: {

    }
}
</script>

<style lang="scss" scoped>
.navbar {
    display: flex;
    flex-wrap: nowrap;
    justify-content: space-between;
    position: fixed;
    bottom: 0;
    box-sizing: border-box;
    border-top: 1px solid #e7e7e7;
    width: 100%;
    height: 45px;
    background-color: #fff;

    .navbar-nav {
        display: flex;
        flex-direction: column;
        justify-content: center;
        width: 20%;
        line-height: 45px;
        text-align: center;
        text-decoration: none;
        color: #5d656b;

        &.router-link-active {
            color: #26a2ff;
        }

        i {
            font-size: 24px;
        }
    }
}
</style>
