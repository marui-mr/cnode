<template>
  <div class="user">
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" alt="">
    </div>
    <div v-else class="userInfo">
      <section class="userIntroduce">
        <img :src="userInfo.avatar_url" alt="">
        <span>{{userInfo.loginname}}</span>
        <p class="score">
          {{userInfo.score}} 积分
        </p>
        <p class="time">
          注册时间 {{userInfo.create_at | formatDate}}
        </p>
      </section>
      <div class="topics">
        <p>最近创建的话题</p>
        <ul>
          <li v-for="(topic,index) in getRecentTopics" :key="index">
            <img :src="topic.author.avatar_url" alt="">
            <router-link :to="{name:'post_content',params:{id:topic.id}}">
              <span>{{topic.title}}</span>
            </router-link>
          </li>
        </ul>
      </div>
      <div class="replies">
        <p>最近参与的话题</p>
        <ul>
          <li v-for="(reply,index) in getRecentReplies" :key="index">
            <img :src="reply.author.avatar_url" alt="">
            <router-link :to="{name:'post_content',params:{id:reply.id}}">
              <span>{{reply.title}}</span>
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
  data(){
    return {
      isLoading:false,
      userInfo:{}
    }
  },
  methods:{
    getData(){
      this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
          .then(res=>{
            if(res.data.success === true){
              this.isLoading = false
              this.userInfo = res.data.data
            }
          })
          .catch(err=>{
            console.log(err)
          })
    }
  },
  beforeMount() {
    this.getData()
    this.isLoading = true
  },
  computed:{
    getRecentTopics(){
      if (this.userInfo.recent_topics.length <= 5){
        return this.userInfo.recent_topics
      }else {
        return this.userInfo.recent_topics.slice(0,5)
      }
    },
    getRecentReplies(){
      if (this.userInfo.recent_replies.length <= 5){
        return this.userInfo.recent_replies
      }else {
        return this.userInfo.recent_replies.slice(0,5)
      }
    }
  }
}
</script>

<style scoped>
.loading{
  width: 400px;
  margin: 0 auto;
}
.user{
  width: 100%;
}
.userInfo .userIntroduce,
.userInfo .topics,
.userInfo .replies{
  margin-top: 15px;
  background-color: #fff;
  border-radius: 3px;
}
.userInfo .userIntroduce{
  padding: 10px 10px 20px;
}
.userInfo .userIntroduce img{
  width: 40px;
  height: 40px;
  border-radius: 4px;
  margin-right: 10px;
  margin-bottom: 15px;
  vertical-align: top;
}
.userInfo .userIntroduce span{
  color: #778087;
  font-size: 14px;
}
.userInfo .userIntroduce .score{
  font-size: 14px;
  color: #333;
}
.userInfo .userIntroduce .time{
  color: #ababab;
  font-size: 14px;
  margin-top: 15px;
}
.userInfo .topics img,
.userInfo .replies img{
  width: 30px;
  height: 30px;
  border-radius: 3px;
  vertical-align: middle;
}
.userInfo .topics p,
.userInfo .replies p{
  color: #444;
  font-size: 14px;
  background-color: #f6f6f6;
  padding: 10px;
  border-radius: 3px 3px 0 0;
}
.userInfo .topics ul ,
.userInfo .replies ul{
  list-style: none;
}
.userInfo .topics ul li,
.userInfo .replies ul li{
  border-top:1px solid #f0f0f0 ;
  padding: 10px;
  display: flex;
  align-items: center;
}
.userInfo .topics ul li span,
.userInfo .replies ul li span{
  color: #08c;
  font-size: 16px;
  margin-left: 10px;
  padding: 3px 0;
}
.userInfo .topics ul li span:hover,
.userInfo .replies ul li span:hover{
  text-decoration: underline;
}
</style>