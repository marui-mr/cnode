<template>
  <div class="slideBar">
    <div v-if="isLoading"></div>
    <div class="authorInfo" v-else>
      <div class="author">
        <div class="topBar">作者</div>
        <div class="authorMain">
          <router-link
            :to="{ name: 'user_info', params: { name: userInfo.loginname } }"
          >
            <img :src="userInfo.avatar_url" alt="" />
          </router-link>
          <span>{{ userInfo.loginname }}</span>
          <p>积分：{{ userInfo.score }}</p>
        </div>
      </div>
      <div class="otherTopics">
        <div class="topBar">作者其他话题</div>
        <ul>
          <li v-for="(list, index) in topicLimit" :key="index">
            <router-link
              :to="{
                name: 'post_content',
                params: { id: list.id, name: list.author.loginname },
              }"
            >
              {{ list.title }}
            </router-link>
          </li>
        </ul>
      </div>
      <div class="replies">
        <div class="topBar">作者最近回复</div>
        <ul>
          <li v-for="(list, index) in replyLimit" :key="index">
            <router-link
              :to="{
                name: 'post_content',
                params: { id: list.id, name: list.author.loginname },
              }"
            >
              {{ list.title }}
            </router-link>
          </li>
        </ul>
      </div>
    </div>
    <div></div>
  </div>
</template>

<script>
export default {
  name: "SlideBar",
  data() {
    return {
      isLoading: false,
      userInfo: {},
    };
  },
  methods: {
    getData() {
      this.$http
        .get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then((res) => {
          if (res.data.success === true) {
            this.isLoading = false;
            this.userInfo = res.data.data;
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  beforeMount() {
    this.isLoading = true;
    this.getData();
  },
  computed: {
    topicLimit() {
      if (this.userInfo.recent_topics) {
        if (this.userInfo.recent_topics.length > 6) {
          return this.userInfo.recent_topics.slice(1, 6);
        } else {
          return this.userInfo.recent_topics.slice(
            1,
            this.userInfo.recent_topics.length
          );
        }
      } else {
        return [];
      }
    },
    replyLimit() {
      if (this.userInfo.recent_replies) {
        if (this.userInfo.recent_replies.length > 5) {
          return this.userInfo.recent_replies.slice(0, 5);
        } else {
          return this.userInfo.recent_replies;
        }
      } else {
        return [];
      }
    },
  },
};
</script>

<style scoped>
ul {
  list-style: none;
}
.slideBar {
  width: 30%;
}
.authorInfo {
  background-color: #fff;
  border-radius: 3px;
  margin-left: 15px;
  margin-top: 15px;
}
.authorInfo .topBar {
  color: #444;
  font-size: 13px;
  background-color: #f6f6f6;
  padding: 10px;
  border-radius: 3px 3px 0 0;
}
.authorInfo .author .authorMain {
  padding: 10px;
}
.authorInfo .author .authorMain img {
  width: 40px;
  height: 40px;
}
.authorInfo .author .authorMain span {
  vertical-align: middle;
  margin-left: 10px;
  color: #778087;
}
.authorInfo .author .authorMain p {
  font-size: 14px;
  color: #333;
  margin-top: 15px;
}
.authorInfo .otherTopics ul,
.authorInfo .replies ul {
  padding: 10px;
}
.authorInfo .otherTopics ul li,
.authorInfo .replies ul li {
  padding: 5px 0;
}
.authorInfo .otherTopics ul li a,
.authorInfo .replies ul li a {
  max-width: 300px;
  display: block;
  color: #778087;
  font-size: 14px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
