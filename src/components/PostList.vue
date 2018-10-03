<template>
<div class="PostList">

 <div class="loading" v-if="IsLoading">
    <img src="../assets/loading.gif">
 </div>

 <div class="posts" v-else>
    <ul>
      <li>
        <div class="topbar">
        <span v-for="(span,index) in spans" :key="span.text" :class="['topic-tab',{active:index === current}]"  @click="changeTopic(index)">{{span.text}}</span>
        </div>
      </li>
        <li v-for="post in posts" :key="post.title">
            <!--头像-->
          <router-link :to="{
          name:'user_info',
          params:{
            name:post.author.loginname
          }
          }">
              <img :src="post.author.avatar_url" alt="">
            </router-link>
            <!--回复/浏览-->
            <span class="allcount">
                <span class="reply_count">{{post.reply_count}}</span>
                /{{post.visit_count}}
            </span>
            <!--帖子的分类-->
            <span :class="[{put_good:(post.good  == true),put_top:(post.top== true),'topiclist-tab':(post.good  != true && post.top  != true)}]">
                <span>
                    {{post | tabFormatter}}
                 </span>
            </span>
            <!--标题-->
            <router-link :to="
              {name:'post_content',
              params:{
              id:post.id,
              name:post.author.loginname
              }}">
                <span class="topic_title">
                  {{post.title}}
                </span>
            </router-link>
            <!--回复时间-->
            <span class="last_reply">
            {{post.last_reply_at | formatDate}}
            </span>
        </li>
        <li>
          <Pagination @handleList="renderList" :currentPage="this.postPage" ref="changePage"></Pagination><!-- //分页 -->
        </li>
    </ul>
  </div>
</div>

</template>

<script>
import Pagination from "./Pagination";
export default {
  name: "postList",
  components: {
    Pagination
  },
  data() {
    return {
      IsLoading: false,
      posts: [],
      postPage: 1,
      limit: 20,
      tab: {},
      spans: [
        { text:"全部" },
        { text: "精华" },
        { text: "分享" },
        { text: "问答" },
        { text: "招聘" }
      ],
      current: 0
    };
  },

  methods: {
    getData(topic) {
      var topicValue;
      if (!topic) {
        topicValue = "";
      }
      topicValue = topic;
      this.$http
        .get("https://cnodejs.org/api/v1/topics", {
          params: {
            page: this.postPage,
            limit: 20,
            tab: topicValue
          }
        })
        .then(res => {
          this.IsLoading = false;
          this.posts = res.data.data;
        })
        .catch(err => {
          console.log(err);
        });
    },
    renderList(value) {
      //this.isLoading = true;
      this.postPage = value[0];
      switch (value[1]) {
        case "全部":
          this.getData();
          break;
        case "精华":
          this.getData("good");
          break;
        case "分享":
          this.getData("share");
          break;
        case "问答":
          this.getData("ask");
          break;
        case "招聘":
          this.getData("job");
          break;
      }
    },
    changeTopic(index) {
      this.current = index;
      this.postPage = 1;
      this.isLoading = true;
      this.$refs.changePage.changePage(); //切换主题过后 重置当前按钮为1
      switch (this.current) {
        case 0:
          this.getData();
          break;
        case 1:
          this.getData("good");
          break;
        case 2:
          this.getData("share");
          break;
        case 3:
          this.getData("ask");
          break;
        case 4:
          this.getData("job");
          break;
      }
    }
  },

  beforeMount() {
    this.IsLoading = true;
    this.getData();
  }
};
</script>

<style scoped>
.PostList {
  background-color: #e1e1e1;
}
.posts {
  margin-top: 10px;
}

.PostList img {
  height: 30px;
  width: 30px;
  vertical-align: middle;
}

ul {
  list-style: none;
  width: 100%;
  max-width: 1344px;
  margin: 0 auto;
}

ul li:not(:first-child) {
  padding: 9px;
  font-size: 15px;
  font-family: "Helvetica Neue", "Luxi Sans", "DejaVu Sans", Tahoma,
    "Hiragino Sans GB", STHeiti, sans-serif !important;
  font-weight: 400;
  background-color: white;
  color: #333;
  border-top: 1px solid #f0f0f0;
  overflow: hidden;
}

li:not(:first-child):hover {
  background: #f5f5f5;
}

li:last-child:hover {
  background: white;
}

li span {
  line-height: 30px;
}

.allcount {
  width: 70px;
  display: inline-block;
  text-align: center;
  font-size: 12px;
}

.reply_count {
  color: #9e78c0;
}

.put_good,
.put_top {
  background: #80bd01;
  padding: 2px 4px;
  border-radius: 3px;
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  -o-border-radius: 3px;
  color: #fff;
  font-size: 12px;
  margin-right: 10px;
}

.topiclist-tab {
  background-color: #e5e5e5;
  color: #999;
  padding: 2px 4px;
  border-radius: 3px;
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  -o-border-radius: 3px;
  font-size: 12px;
  margin-right: 10px;
}

.topic_title {
  max-width: 70%;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
  display: inline-block;
  vertical-align: middle;
  font-size: 16px;
  line-height: 30px;
  color: #333;
}

.topic_title:hover {
  white-space: normal;
  overflow: auto;
}

.last_reply {
  text-align: right;
  min-width: 50px;
  display: inline-block;
  white-space: nowrap;
  float: right;
  color: #778087;
  font-size: 12px;
}

.topbar {
  height: 40px;
  background-color: #f5f5f5;
}

.topbar span {
  font-size: 14px;
  color: #80bd01;
  line-height: 40px;
  margin: 0 10px;
  cursor: pointer;
}

.topbar span:hover {
  color: #9e78c0;
}

a {
  text-decoration: none;
  color: black;
}

a:hover {
  text-decoration: underline;
}

.loading {
  text-align: center;
  padding-top: 300px;
}
.topic-tab.active {
  background-color: #80bd01;
  color: #fff;
  padding: 3px 4px;
  border-radius: 3px;
}
</style>