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
        {{ article.time | moment }}
        {{ article.type }}
        阅读（{{ article.visitedAmount }}）
      </div>
    </div>
    <div class="comment-area">
      <!-- 评论列表 -->
      <div class="comment-publish" v-if="logined">
        <div class="comment-publish-header">
          <router-link :to="'/people/' + userInfo.id">{{ userInfo.username }}</router-link>
          <span>{{ userInfo.word }}</span>
        </div>
        <textarea name="comment-publish-text" id="comment-publish-text" v-model="commentPublishText" @focus="conmmentFocus"></textarea>
        <button class="primary" v-on:click="publishComment">提交</button>
      </div>
      <div v-else>
        <router-link :to="'/login'">去登陆</router-link>
      </div>
      <div class="comment-list">
        <div class="comment-list-title">
          {{ comments.length }}条评论
        </div>
        <div class="comment-content" v-for="comment in comments" :key="comment.id">
          {{ comment.user.username }}
          {{ comment.time | moment }}
          {{ comment.text }}
        </div>
      </div>
    </div>
    <div class="article-footer">
      <!-- 功能区 -->
      <div class="navbar-nav">
        <i class="fa fa-gift"></i>
        <a href="javascript:;">赞赏</a>
      </div>
      <div class="navbar-nav">
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
import axios from 'axios';
import {config} from '../assets/scripts/config.js';

export default {
  name: 'article',
  data(){
    return {
      msg: 'article',
      logined: false,
      userInfo: {},
      article: {
        user: {}
      },
      comments: [],
      commentPublishText: '写下你的观点...'
    }
  },
  methods: {
    getArticle(){
      this.$ajax.get('http://' + config.host + ':' + config.port +'/articles?id=' + this.$route.params.id + '&_expand=user')
        .then(function(response){
          console.log('1');
          console.log(JSON.stringify(response.data));
          this.article = response.data[0];
          this.visitArticle();
        }.bind(this))
        .catch(function(error){
          console.log(error);
        });
    },
    getComments(){
      this.$ajax.get('http://' + config.host + ':' + config.port + '/articles/' + this.$route.params.id + '/comments?&_expand=user')
        .then(function(response){
          this.comments = response.data;
          console.log(response.data);
        }.bind(this))
        .catch(function(error){
          console.log(error);
        });
    },
    getUserInfo: function(){
      if(this.$store.state.logined){
        this.logined = true;
      }

      if(localStorage.userInfo){
        this.userInfo = JSON.parse(localStorage.userInfo);
      }
    },
    publishComment: function(){
      var newComment = {
        time: Date.now(),
        text: this.commentPublishText,
        userId: Number(this.userInfo.id),
        articleId: Number(this.$route.params.id)
      };
      this.$ajax.post('http://'+ config.host + ':' + config.port + '/comments', newComment)
        .then(function(response){
          console.log(response);
          this.getComments();
        }.bind(this)).catch(function(error){
          console.log(error);
        });
      console.log(this.commentPublishText);
      // this.article.comments.push(this.commentPublishText);
    },
    visitArticle(){
      var _this = this;
      // 访问量增加1
      axios.patch('http://'+ config.host + ':' + config.port + '/articles/' + _this.$route.params.id, {
        visitedAmount: ++ _this.article.visitedAmount
      })
        .then(function(response){
          console.log(response);
        }.bind(this)).catch(function(error){
          console.log(error);
        });
    },
    conmmentFocus(){
      this.commentPublishText = '';
    }
  },
  created(){
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

        &+span {
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

    .comment-publish {
      padding-top: 16px;
      border-top: 1px solid #888;

      .comment-publish-header {
        margin-bottom: 12px;

        a {
          text-decoration-line: none;
          color: #2b2c2d;
        }

        span {
          font-size: 12px;
          color: #888;
        }
      }

      textarea {
        display: block;
        padding: 12px 16px;
        box-sizing: border-box;
        width: 100%;
        height: 100px;
      }
    }
    .comment-list {

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
