<template>
    <div class="main">
        <!-- {{ $route.params.id }} -->
        <div class="article-content">
            <!-- 文章内容 -->
            <div class="article-header">
                <!-- 头像，用户名等 -->
                <router-link :to="'/people/' + article.user.id">{{ article.user.username }}</router-link>
                <span>{{ article.user.constellation }}</span>
                <!-- <span>{{ aritcle.type }}</span> -->
            </div>
            <div class="article-text">
                <!-- 正文内容 -->
                <!-- <router-link :to="'/'">{{ article.title }}</router-link> -->
                <p>
                    {{ article.title }}
                </p>
            </div>
            <div class="article-footer">
                <!-- 时间，统计信息，点赞回复等 -->
                {{ article.time | moment }} {{ article.type }} 阅读（{{ article.visitedAmount }}）
            </div>
        </div>
        <div class="comment-area">
            <!-- 评论列表 -->

            <div class="comment-list">
                <div class="comment-list-title">
                    {{ comments.length }}条评论
                </div>
                <div class="comment-content" v-for="comment in comments" :key="comment.id">
                    <div class="comment-title">
                        <div class="comment-userheader">
                            <img :src="comment.user.portrait" alt="portrait">
                        </div>
                        <div class="comment-userinfo">
                            <div class="comment-username">
                                {{ comment.user.username }}
                            </div>
                            <div class="comment-time">
                                {{ comment.time | moment }}
                            </div>
                        </div>
                    </div>
                    <div class="comment-text">
                        {{ comment.text }}
                    </div>
                </div>
            </div>
        </div>
        <div class="comment-input-area" v-if="commentInputVisible">
            <input type="text" @focus="commentInputFocus" placeholder="写下你的评论...">
        </div>

        <!-- 发表评论 -->
        <div class="comment-publish" v-if="logined && commentTextareaVisible" @click="hideCommentTextarea">
            <div class="comment-publish-box">
                <textarea name="comment-publish-text" id="comment-publish-text" v-model="commentPublishText" ref="commentTextarea" placeholder="写下你的观点..."></textarea>
                <button class="primary" v-on:click="publishComment">发表评论</button>
            </div>
        </div>
        <div v-else>
            <router-link :to="'/login'">去登陆</router-link>
        </div>
        <div class="article-footer">
            <!-- 功能区 -->
            <div class="navbar-nav">
                <i class="fa fa-gift"></i>
                <a href="javascript:;">赞赏</a>
            </div>
            <div class="navbar-nav" @click="showCommentInput">
                <i class="fa fa-comment-o"></i>
                <a href="javascript:;">评论</a>
            </div>
            <div class="navbar-nav">
                <i class="fa fa-heart-o"></i>
                <a href="javascript:;">喜欢</a>
            </div>
            <div class="navbar-nav">
                <i class="fa fa-share-alt"></i>
                <a href="javascript:;">分享</a>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    name: 'articleList',
    data () {
        return {
            msg: 'article',
            userInfo: {},
            article: {
                user: {}
            },
            comments: [],
            commentPublishText: '',
            commentInputVisible: false,
            commentTextareaVisible: false
        }
    },
    computed: {
        logined () {
            var logined = localStorage.getItem('logined');
            return this.$store.state.logined || logined;
        }
    },
    methods: {
        getArticle () {
            this.$ajax.get('/api/articles?id=' + this.$route.params.id + '&_expand=user')
                .then(function (response) {
                    console.log('1');
                    console.log(JSON.stringify(response.data));
                    this.article = response.data[0];
                    this.visitArticle();
                }.bind(this))
                .catch(function (error) {
                    console.log(error);
                });
        },
        getComments () {
            this.$ajax.get('/api/articles/' + this.$route.params.id + '/comments?&_expand=user')
                .then(function (response) {
                    this.comments = response.data;
                    console.log(response.data);
                }.bind(this))
                .catch(function (error) {
                    console.log(error);
                });
        },
        getUserInfo: function () {
            if (this.$store.state.logined || localStorage.userInfo) {
                this.userInfo = JSON.parse(localStorage.userInfo);
            }
        },
        showCommentInput () {
            this.commentInputVisible = !this.commentInputVisible
        },
        commentInputFocus () {
            this.commentTextareaVisible = true
            this.$nextTick(function () {
                this.$refs.commentTextarea.focus()
            })
        },
        hideCommentTextarea () {
            this.commentTextareaVisible = false
        },
        publishComment: function () {
            var newComment = {
                time: Date.now(),
                text: this.commentPublishText,
                userId: Number(this.userInfo.id),
                articleId: Number(this.$route.params.id)
            };
            this.$ajax.post('/api/comments', newComment)
                .then(function (response) {
                    console.log(response);
                    this.getComments();
                }.bind(this)).catch(function (error) {
                    console.log(error);
                });
            console.log(this.commentPublishText);
            // this.article.comments.push(this.commentPublishText);
        },
        visitArticle () {
            var _this = this;
            // 访问量增加1
            this.$ajax.patch('/api/articles/' + _this.$route.params.id, {
                visitedAmount: ++_this.article.visitedAmount
            })
                .then(function (response) {
                    console.log(response);
                }.bind(this)).catch(function (error) {
                    console.log(error);
                });
        }
    },
    created () {
        this.getArticle();
        this.getComments();
        this.getUserInfo();
    }
}
</script>

<style lang="scss" scoped>
.main {
    .article-content {
        margin-bottom: 24px;
        padding: 0 14px;

        .article-header {
            margin-bottom: 12px;

            a {
                text-decoration-line: none;
                color: #2b2c2d;

                & + span {
                    font-size: 12px;
                    color: #888;
                }
            }
        }

        .article-text {
            margin-bottom: 10px;
            font-size: 14px;

            p {
                line-height: 1.75em;
                font-size: 15px;
                color: #262626;
            }
        }

        .article-footer {
            font-size: 12px;
            color: #888;
        }
    }

    .comment-area {
        padding: 0 14px;

        .comment-list {
            .comment-list-title {
            }
            .comment-content {
                margin-top: 14px;
                .comment-title {
                    display: flex;

                    .comment-userheader {
                        width: 50px;

                        img {
                            display: block;
                            width: 40px;
                            height: 40px;
                            border-radius: 50%;
                        }
                    }

                    .comment-userinfo {
                        flex: 1;

                        .comment-username {
                            font-size: 14px;
                            color: #101010;
                        }

                        .comment-time {
                            margin-top: 8px;
                            font-size: 12px;
                            color: #999;
                        }
                    }
                }
                .comment-text {
                    margin-top: 10px;
                    font-size: 14px;
                }
            }
        }
    }

    // 评论输入框
    .comment-input-area {
        position: fixed;
        right: 8px;
        bottom: 45px;
        left: 8px;
        height: 25px;

        input {
            display: block;
            width: 100%;
            height: 25px;
            line-height: 25px;
            background-color: #f2f2f2;
            border: none;
        }
    }

    // 发表评论
    .comment-publish {
        position: fixed;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        background-color: rgba(0, 0, 0, 0.5);

        .comment-publish-box {
            position: fixed;
            right: 0;
            bottom: 45px;
            left: 0;
            padding: 16px 14px 10px;
            background-color: #fff;

            textarea {
                display: block;
                padding: 10px;
                box-sizing: border-box;
                width: 100%;
                height: 100px;
            }

            button {
                float: right;
                margin-top: 8px;
                padding: 0 10px;
                border: 1px solid #0f88eb;
                background-color: #fff;
                line-height: 24px;
                color: #0f88eb;
            }
        }
    }

    /* 功能区 */
    .article-footer {
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
            color: #5d656b;

            i {
                // font-size: 24px;
            }

            a {
                display: inline-block;
                height: 20px;
                line-height: 20px;
                text-decoration: none;
                font-size: 12px;
                color: #5d656b;
            }
        }
    }
}
</style>
