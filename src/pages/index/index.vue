<template>
  <div>
    <nav class="nav">
      <ul flex="box:mean">

        <li v-for="item in tabs" :class="{ active: item.tab === ($route.query.tab || '') }">
          <router-link :to="{ name: 'index', query: { tab: item.tab } }">{{ item.title }}</router-link>
        </li>
      </ul>
    </nav>
    <v-content v-vuet-scroll="{ path: 'topic-list', name: 'content' }">
      <ul class="list">
        <li v-for="item in list.data" key="item.id">
          <router-link :to="{ name: 'topic-detail', params: { id: item.id } }">
            <div class="top" flex="box:first">
              <div class="headimg" :style="{ backgroundImage: 'url(' + item.author.avatar_url + ')' }"></div>
              <div class="box" flex="dir:top">
                <strong>{{ item.author.loginname }}</strong>
                <div flex>
                  <time>{{ item.create_at | formatDate }}</time>
                  <span class="tag">#分享#</span>
                </div>
              </div>
            </div>
            <div class="common-typeicon" flex v-if="item.top || item.good">
              <div class="icon" v-if="item.good">
                <i class="iconfont icon-topic-good"></i>
              </div>
              <div class="icon" v-if="item.top">
                <i class="iconfont icon-topic-top"></i>
              </div>
            </div>
            <div class="tit">{{ item.title }}</div>
            <div class="expand" flex="box:mean">
              <div class="item click" flex="main:center cross:center">
                <i class="iconfont icon-click"></i>
                <div class="num">{{ item.visit_count > 0 ? item.visit_count : '暂无阅读' }}</div>
              </div>
              <div class="item reply" flex="main:center cross:center">
                <i class="iconfont icon-comment"></i>
                <div class="num">{{ item.reply_count > 0 ? item.reply_count : '暂无评论' }}</div>
              </div>
              <div class="item last-reply" flex="main:center cross:center">
                <time class="time">{{ item.last_reply_at | formatDate }}</time>
              </div>
            </div>
          </router-link>
        </li>
      </ul>
      <v-loading :done="list.done" :loading="list.loading" @seeing="$vuet.fetch('topic-list')"></v-loading>
    </v-content>
    <v-footer></v-footer>
  </div>
</template>
<script>
  import { mapModules, mapRules } from 'vuet'

  export default {
    mixins: [
      mapModules({ list: 'topic-list' }),
      mapRules({ route: 'topic-list' })
    ],
    data () {
      return {
        tabs: [
          {
            title: '全部',
            tab: ''
          },
          {
            title: '精华',
            tab: 'good'
          },
          {
            title: '分享',
            tab: 'share'
          },
          {
            title: '问答',
            tab: 'ask'
          },
          {
            title: '招聘',
            tab: 'job'
          }
        ]
      }
    }
  }
</script>
<style lang="less" scoped>
  @import "../../less/config";
  .nav {
    position: absolute;
    background: #fff;
    top: 0;
    right: 0;
    left: 0;
    z-index: 10;
    border-bottom: 1px solid #ddd;
    ul {
      overflow: hidden;
      padding: 0;
      margin: 0;
      li {
        position: relative;
        list-style: none;
        line-height: 49px;
        text-align: center;
      }
      a {
        display: block;
        text-decoration: none;
        font-size: 14px;
        color: lighten(@text, 50%);
      }
      .active {
        &:after {
          content: "";
          position: absolute;
          right: 0;
          left: 0;
          bottom: 1px;
          z-index: 1;
          height: 3px;
          background: @main;
        }
        a {
          color: @text;
        }
      }
    }
  }


  .list {
    overflow: hidden;
    padding: 0;
    margin: 0;
    background: #eee;
    li {
      position: relative;
      padding: 15px 15px 0 15px;
      margin-bottom: 15px;
      list-style: none;
      box-shadow: 0 0 5px #ccc;
      background: #fff;
    }
    .top {
      height: 40px;
      .headimg {
        overflow: hidden;
        width: 30px;
        height: 30px;
        border-radius: 50%;
        border: 1px solid #ddd;
        background-position: center center;
        background-size: cover;
      }
      .box {
        padding-left: 5px;
        strong {
          line-height: 24px;
          font-size: 16px;
          font-weight: normal;
          color: darken(@text, 10%);
        }
        time {
          line-height: 16px;
          font-size: 12px;
          font-style: normal;
          color: #aaa;
        }
        .tag {
          margin-left: 4px;
          line-height: 16px;
          font-size: 12px;
          font-style: normal;
          color: @main;
        }
      }
    }
    .tit {
      padding: 10px 0;
      line-height: 22px;
      font-size: 16px;
      font-weight: bold;
      color: @text;
    }
  }


  .expand {
    padding: 10px 0;
    border-top: 1px solid #e1e1e1;
    text-align: center;
    .item {
      padding: 0 10px;
      line-height: 20px;
      text-align: center;
      border-right: 1px solid #e1e1e1;
      &:last-of-type {
        border: none;
      }
      .iconfont {
        color: #aaa;
      }
      .num,
      .time {
        padding-left: 3px;
        font-size: 12px;
        color: #aaa;
      }
      .pic,
      img {
        width: 16px;
        height: 16px;
        background: #e1e1e1;
        background-size: cover;
        background-position: center center;
      }
      .pic {
        overflow: hidden;
        border-radius: 50%;
      }
    }
  }
</style>
