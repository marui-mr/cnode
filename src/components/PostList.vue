<template>
  <div class="list">
    <!--数据未返回的时候显示这个正在加载的动图-->
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" alt="">
    </div>
    <!--代表我们的主题帖子列表-->
    <div class="postList" v-else>
      <ul>
        <li>
          <div class="topBar">
            <span>全部</span>
            <span>精华</span>
            <span>分享</span>
            <span>问答</span>
            <span>招聘</span>
          </div>
        </li>
        <li v-for="(post,index) in posts" :key="index">
          <!--头像-->
          <img :src="post.author.avatar_url" alt="">
          <!--回复/浏览-->
          <span>
            <span class="reply_count">{{post.reply_count}}</span>
            <span class="separate">/</span>
            <span class="visit_count">{{post.visit_count}}</span>
          </span>
          <!--类型-->
          <span :class="[{'put_good':post.good === true,'put_top':post.top === true,'topiclist_tab':post.good !== true && post.top !== true}]">
            <span>
              {{post | formatType}}
            </span>
          </span>
          <!--标题-->
          <router-link :to="{name:'post_content',params:{id:post.id,name:post.author.loginname}}">
            <span class="title">
              {{post.title}}
            </span>
          </router-link>
          <!--最终回复时间-->
          <span class="time">
            {{post.last_reply_at | formatDate}}
          </span>
        </li>
        <!--分页-->
        <li>
          <pagination @handleList="renderList"></pagination>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import pagination from './Pagination'
export default {
  name: "PostList",
  data(){
    return {
      isLoading:false,
      posts:[],  // 代表页面的列表数组
      postPage:1
    }
  },
  components:{
    pagination
  },
  methods:{
    getData(){
      this.$http.get('https://cnodejs.org/api/v1/topics',{
        params:{
          page:this.postPage,
          limit:20
        }
      }).then(res=>{
        if (res.data.success === true){
          this.isLoading = false // 加载成功去除动画
          this.posts = res.data.data
        }
      }).catch(err=>{
        console.log(err)
      })
    },
    renderList(value){
      this.postPage = value
      this.getData()
    }
  },
  beforeMount() {
    this.isLoading = true  // 加载成功之前显示加载动画
    this.getData()  // 在页面加载之前获取数据
  }
}
</script>

<style scoped>
.loading{
  width: 400px;
  margin: 0 auto;
}
.list{
  width: 100%;
}
.postList{
  background-color: #fff;
}
.postList .topBar{
  background-color: #f6f6f6;
  height: 40px;
  display: flex;
  align-items: center;
}
.postList .topBar span{
  margin: 0 15px;
  color: #80bd01;
  font-size: 16px;
  cursor: pointer;
}
.postList .topBar span:hover{
  color: #08c;
}
.postList ul{
  list-style: none;
}
.postList ul li{
  position: relative;
  border-top: 1px solid #f0f0f0;
}
.postList ul li:not(:first-child){
  height: 50px;
  display: flex;
  align-items: center;
}
.postList ul li:not(:first-child):hover{
  background-color: #f6f6f6;
}
.postList img{
  width: 30px;
  height: 30px;
  border-radius: 3px;
  margin:0 8px;
}
.postList .reply_count{
  color: #9e78c0;
  font-size: 14px;
}
.postList .separate{
  font-size: 10px;
  margin: 0 1px;
}
.postList .visit_count{
  font-size: 10px;
  color: #b4b4b4;
}
.postList .put_good,
.postList .put_top,
.postList .topiclist_tab{
  font-size: 12px;
  border-radius: 3px;
  padding:1px 3px;
  margin: 0 5px;
}
.postList .put_good,
.postList .put_top{
  background-color: #80bd01;
  color: white;
}
.postList .topiclist_tab{
  background-color: #e5e5e5;
  color: #999;
}
.postList .title{
  color: #333;
  font-size: 16px;
}
.postList .title:hover{
  text-decoration: underline;
}
.postList .time{
  color: #778087;
  font-size: 11px;
  position:absolute ;
  right: 8px;
}
</style>