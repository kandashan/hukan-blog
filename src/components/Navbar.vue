<template>
    <div class="navbar">
        <router-link class="navbar-nav" to="/" exact>
            <i class="navbar-icon navbar-icon-home"></i>
        </router-link>
        <router-link class="navbar-nav" to="/articleTypes">
            <i class="navbar-icon navbar-icon-find"></i>
        </router-link>
        <router-link class="navbar-nav" to="/photos">
            <i class="navbar-icon navbar-icon-message"></i>
        </router-link>
        <router-link class="navbar-nav" to="/mine">
            <i class="navbar-icon navbar-icon-mine"></i>
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
        justify-content: center;
        align-items: center;
        width: 20%;
        line-height: 45px;
        text-align: center;
        text-decoration: none;
        color: #5d656b;

        &.router-link-active {
            color: #26a2ff;

            .navbar-icon-home {
                background-image: url('./../assets/images/icon_tabbar_home_active.png');
            }

            .navbar-icon-find {
                background-image: url('./../assets/images/icon_tabbar_find_active.png');
            }

            .navbar-icon-message {
                background-image: url('./../assets/images/icon_tabbar_message_active.png');
            }

            .navbar-icon-mine {
                background-image: url('./../assets/images/icon_tabbar_mine_active.png');
            }
        }

        i {
            display: block;
            width: 30px;
            height: 30px;

            background-repeat: no-repeat;
            background-position: center;
            background-size: 30px;

            &.navbar-icon-home {
                background-image: url('./../assets/images/icon_tabbar_home.png');
            }

            &.navbar-icon-find {
                background-image: url('./../assets/images/icon_tabbar_find.png');
            }

            &.navbar-icon-message {
                background-image: url('./../assets/images/icon_tabbar_message.png');
            }

            &.navbar-icon-mine {
                background-image: url('./../assets/images/icon_tabbar_mine.png');
            }
        }
    }
}
</style>
