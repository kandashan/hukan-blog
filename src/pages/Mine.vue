<template>
    <div class="main">
        <Blogheader>
            <span class="header-title">我的</span>
        </Blogheader>
        <div class="mine-info">
            <router-link class="mine-info-person"
                         v-if="logined"
                         to="/userInfo">
                <div class="mine-info-person-header">
                    <img :src="userInfo.portrait"
                         alt="header">
                </div>
                <div class="mine-info-person-text">
                    <div class="mine-info-person-text-title">
                        <span>{{ userInfo.username }}</span>
                        <span>{{ userInfo.constellation }}</span>
                    </div>
                    <div class="mine-info-person-text-content">
                        <p>{{ userInfo.word }}</p>
                    </div>
                </div>
            </router-link>
            <div v-else>
                <router-link to="/login">点击登陆</router-link>
            </div>
            <div class="mine-info-article">
                <div>
                    0
                    <br> 文章
                </div>
                <div>
                    1
                    <br> 关注
                </div>
                <div>
                    32
                    <br> 粉丝
                </div>
            </div>
        </div>
        <div class="mine-article-info">
            <ul>
                <li>
                    <i class="fa fa-lock fa-fw"></i> 私密文章
                    <span>5</span>
                </li>
                <li>
                    <i class="fa fa-bookmark fa-fw"></i> 收藏的文章
                    <span>5</span>
                </li>
                <li>
                    <i class="fa fa-heart fa-fw"></i> 喜欢的文章
                    <span>45</span>
                </li>
            </ul>
        </div>
        <navbar></navbar>
    </div>
</template>

<script>
import Blogheader from '../components/Blogheader'
import Navbar from '../components/Navbar'

export default {
    name: 'mine',
    data () {
        return {
            msg: '我的',
            logined: false,
            userInfo: {}
        }
    },
    methods: {
        getUserInfo: function () {
            if (localStorage.userInfo) {
                this.logined = true;
                this.userInfo = JSON.parse(localStorage.userInfo);
            }
        }
    },
    created () {
        this.getUserInfo();
    },
    components: {
        Blogheader,
        Navbar
    }
}
</script>

<style lang="scss" scoped>
.main {
    background-color: #f2f2f2;

    .mine-info {
        margin-top: 67px;
        margin-bottom: 10px;

        .mine-info-person {
            display: flex;
            justify-content: center;
            height: 70px;
            background-color: #fff;
            text-decoration: none;

            .mine-info-person-header {
                padding: 10px;
                width: 70px;

                img {
                    width: 50px;
                    height: 50px;
                    border-radius: 50%;
                }
            }

            .mine-info-person-text {
                flex: 1;

                .mine-info-person-text-title {
                    margin-top: 14px;

                    span {
                        &:first-of-type {
                            font-size: 16px;
                            color: #101010;
                        }

                        &:last-of-type {
                            font-size: 12px;
                            color: #999;
                        }
                    }
                }

                .mine-info-person-text-content {
                    margin-top: 4px;

                    p {
                        font-size: 14px;
                        color: #666;
                    }
                }
            }
        }

        .mine-info-article {
            display: flex;
            justify-content: space-around;
            align-items: center;
            margin-bottom: 10px;
            padding: 14px 0;
            border-top: 1px solid #e1e1e1;
            border-bottom: 1px solid #e1e1e1;
            background-color: #fff;

            div {
                line-height: 25px;
                text-align: center;
            }
        }
    }

    .mine-article-info {
        border-top: 1px solid #e1e1e1;
        background-color: #fff;

        ul {
            list-style: none;

            li {
                padding: 0 10px;
                height: 40px;
                line-height: 40px;
                font-size: 14px;
                border-bottom: 1px solid #e7e7e7;

                i {
                    font-size: 20px;
                }

                span {
                    float: right;
                }
            }
        }
    }
}
</style>
