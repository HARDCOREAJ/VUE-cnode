<template>
<div class="">
    <div class="loading" v-if="isLoading">
        <img src="../assets/loading.gif">
    </div>
    <div class="userInfomation" v-else>
        <section>
            <img :src="userinfo.avatar_url" alt="">
            <span>{{userinfo.loginname}}</span>
            <p>{{userinfo.score}}积分</p>
            <p>注册时间：{{userinfo.create_at | formatDate}}</p>
        </section>
        <div class="replyies">
        <p>回复主题</p>
        <ul>
            <li v-for="(item,index) in userinfo.recent_replies" :key="index">
               <!--头像-->
                <router-link :to="{name:'post_content',params:{id:item.id}}" >
                    {{item.title}}
                </router-link>
            </li> 
        </ul>

        </div>
        <div class="topics">
        <p>最近参与话题</p>
        <ul>
            <li v-for="(item,index) in userinfo.recent_topics" :key="index">
                <router-link :to="{name:'post_content',params:{id:item.id}}" >
                    {{item.title}}
                </router-link>
            </li> 
        </ul>
        </div>
    </div>
</div>
</template>

<script>
export default {
  name: "UserInfo",
  data() {
    return {
      isLoading: false,
      userinfo: {}
    };
  },
  methods: {
    getData() {
      this.$http
        .get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`) //接受来自PostList或Aricle&Slidebar的params的name做为请求需要的后缀参数
        .then(res => {
          if (res.data.success == true) {
            this.isLoading = false;
            console.log(res);
            this.userinfo = res.data.data;
          }
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  beforeMount() {
    this.isLoading = true;
    this.getData();
  }
};
</script>

<style scoped>
.userInfomation {
  background: white;
  width: 75%;
  margin: 10px auto;
}
.userInfomation section {
  padding: 12px;
}
.userInfomation img {
  width: 30px;
}
.userInfomation li {
  padding: 9px;
  font-size: 15px;
  font-family: "Helvetica Neue", "Luxi Sans", "DejaVu Sans", Tahoma,
    "Hiragino Sans GB", STHeiti, sans-serif !important;
  font-weight: 400;
  background-color: white;
  color: #333;
  border-top: 1px solid #f0f0f0;
  list-style: none;
}
.userInfomation .replies,
.userInfomation .topics {
  font-size: 0.72rem;
  border-top: 10px #dddddd solid;
}
.userInfomation > div > p {
  padding: 12px 0 12px 12px;
  background-color: rgba(212, 205, 205, 0.17);
  font-size: 0.75rem;
  margin: 0;
}
.userInfomation > div > ul > li {
  padding: 4px 0 4px 12px;
  white-space: nowrap;
  font-size: 0.72rem;
  text-overflow: ellipsis;
  overflow: hidden;
  line-height: 30px;
  vertical-align: middle;
}
.userInfomation > div > ul > li > a {
  color: #094e99;
  text-decoration: none;
}
</style>