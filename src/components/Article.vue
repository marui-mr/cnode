<template>
  <div class="article">
    <!--如果正在加载显示此div-->
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" alt="" />
    </div>
    <div v-else>
      <div class="topic_header">
        <div class="topic_title">{{ post.title }}</div>
        <ul class="topic_introduce">
          <li>发布于 {{ post.create_at | formatDate }}</li>
          <li>作者 {{ post.author.loginname }}</li>
          <li>{{ post.visit_count }} 次浏览</li>
          <li>来自 {{ post | formatType }}</li>
        </ul>
        <div
          id="content"
          v-html="post.content"
          class="topic_content"
          v-highlight
        ></div>
      </div>
      <div class="reply">
        <div class="topbar">回复</div>
        <div
          v-for="(reply, index) in post.replies"
          :key="index"
          class="reply_info"
        >
          <router-link
            :to="{
              name: 'user_info',
              params: { name: reply.author.loginname },
            }"
          >
            <img :src="reply.author.avatar_url" alt="" />
          </router-link>
          <div class="reply_info_header">
            <router-link
              :to="{
                name: 'user_info',
                params: { name: reply.author.loginname },
              }"
            >
              <span class="name">{{ reply.author.loginname }}</span>
            </router-link>
            <span class="floor">{{ index + 1 }}楼</span>
          </div>
          <span v-if="reply.ups.length > 0" class="up_count"
            >点赞:{{ reply.ups.length }}</span
          >
          <span v-else></span>
          <p v-html="reply.content" class="reply_content"></p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Article",
  data() {
    return {
      isLoading: false,
      post: {}, // 代表当前文章页的所有内容，所有属性
    };
  },
  methods: {
    getArticleData() {
      this.$http
        .get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`)
        .then((res) => {
          if (res.data.success === true) {
            this.post = res.data.data;
            this.isLoading = false;
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  beforeMount() {
    this.isLoading = true;
    this.getArticleData();
  },
  watch: {
    $route() {
      this.getArticleData();
    },
  },
};
</script>

<style>
@import url("../assets/markdown-github.css");
</style>

<style scoped>
.loading {
  width: 400px;
  margin: 0 auto;
}
.article {
  width: 70%;
}
.article .topic_header,
.article .reply {
  margin-top: 15px;
  background-color: #fff;
  border-radius: 3px;
}
.article .topic_header .topic_title {
  font-size: 22px;
  font-weight: 700;
  margin-top: 10px;
  padding: 5px 5px;
}
.article .topic_header .topic_introduce {
  display: flex;
  padding: 5px 0 15px;
  border-bottom: 1px solid #e5e5e5;
  list-style: none;
}
.article .topic_header .topic_introduce li {
  font-size: 12px;
  color: #838383;
  margin-left: 5px;
}
.article .topic_header .topic_introduce li::before {
  content: "";
  display: inline-block;
  width: 4px;
  height: 4px;
  border-radius: 50%;
  background-color: #838383;
  position: relative;
  bottom: 2px;
  left: 0;
  margin-right: 4px;
}
.article .topic_header .topic_content {
  padding: 15px 20px;
}
.article .reply .topbar {
  color: #444;
  font-size: 14px;
  background-color: #f6f6f6;
  padding: 10px;
  border-radius: 3px 3px 0 0;
}
.article .reply .reply_info {
  border-top: 1px solid #f0f0f0;
  padding: 10px;
  position: relative;
}
.article .reply .reply_info img {
  width: 30px;
  height: 30px;
  border-radius: 3px;
}
.article .reply .reply_info .reply_info_header {
  position: absolute;
  top: 7px;
  left: 40px;
}
.article .reply .reply_info .reply_info_header .name {
  margin: 0 10px;
  color: #666;
  font-size: 12px;
  font-weight: 700;
}
.article .reply .reply_info .reply_info_header .floor {
  font-size: 11px;
  color: #08c;
}
.article .reply .reply_info .up_count {
  position: absolute;
  top: 10px;
  right: 10px;
  color: gray;
  font-size: 15px;
}
.article .reply .reply_info .reply_content {
  color: #333;
  font-size: 15px;
  line-height: 1.7em;
  padding: 0 60px;
  margin-top: -10px;
  margin-bottom: 10px;
}
</style>
