<template>
  <div class="hello">
    <div id="content" style="margin-right: 0;text-align: left">
      <div class="panel">
        <div class="header clear " >
          <a class="topic-tab cu "  v-bind:class="{ active:tit.style }" v-for="(tit, index) in title" @click="tab(index)">{{tit.name}}</a>
          <div class="clear"></div>
        </div>
        <div class="inner no-padding">
          <div id="topic_list" v-for="x in posts.data">
            <div class="cell">
              <a class="user_avatar pull-left" href="/user/i5ting">
                <img v-bind:src=x.author.avatar_url v-bind:title=x.author.loginname>
              </a>
              <span class="reply_count pull-left">
                <span class="count_of_replies" title="回复数">{{x.reply_count}}</span>
                <span class="count_seperator">/</span>
                <span class="count_of_visits" title="点击数">{{x.visit_count}}</span></span>
              <a class="last_time pull-right" href="">
                <img class="user_small_avatar" src="https://avatars3.githubusercontent.com/u/26704801?v=3&amp;s=120">
                <span class="last_active_time">7 小时前</span></a>
              <div class="topic_title_wrapper">
                <span class="put_top" v-if="x.top">置顶</span>
                <span class="put_top" v-if="x.good">精华</span>
                <router-link :to="{ name: 'Content', query: {id: x.id}}">
                  <a class="topic_title"  href="javascript:;"  v-bind:title=x.title >{{x.title}}</a>
                </router-link>
              </div>
            </div>
          </div>

        </div>
      </div>
    </div>
  </div>
</template>
<script>
  export default {
    data () {
      return {
        posts: [{}],
        num: 40,
        title: [
          {name: '全部', type: 'all', style: true},
          {name: '精华', type: 'good', style: false},
          {name: '分享', type: 'share', style: false},
          {name: '问答', type: 'ask', style: false},
          {name: '招聘', type: 'all', style: false},
          {name: '客户端测试', type: 'dev', style: false}
        ]
      }
    },
    mounted: function () {
      // GET request
      this.$http.get('https://cnodejs.org/api/v1/topics').then(
        (data) => {
          this.posts = data.body
        },
        (data) => {
          console.log(data)
        })
    },
    methods: {
      tab: function (key) {
        for (let x in this.title) {
          this.title[x].style = false
        };
        let tab = this.title[key].type
        this.title[key].style = true
        this.$http.get('https://cnodejs.org/api/v1/topics?tab=' + tab).then(
          (data) => {
            console.log(data, key)
            this.posts = data.body
          },
          (data) => {
          })
      }
    }
  }
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1, h2 { font-weight: normal; }
  ul { list-style-type: none; padding: 0; }
  li { display: inline-block; margin: 0 10px; }
  a { color: #42b983; }
  .active{
    background-color: #80bd01;
    color: #fff;
    padding: 3px 4px;
    border-radius: 3px
  }
</style>
