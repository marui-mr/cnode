<template>
  <div class="header">
    <div class="container">
      <span class="logo">
        <router-link :to="{name:'root'}">
          <img src="../assets/cnodejs_light.svg" alt="">
        </router-link>
      </span>
      <ul>
        <li><a href="#">首页</a></li>
        <li><a href="#">新手入门</a></li>
        <li><a href="#">API</a></li>
        <li><a href="#">关于</a></li>
        <li><a href="#">注册</a></li>
        <li><a href="#">登录</a></li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "Header"
}
</script>

<style scoped>
.header{
  height: 50px;
  background-color: #444;
}
.header .container{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  width: 80%;
  margin:0 auto ;
}
.logo{
  width: 120px;
}
ul{
  height: 50px;
  display: flex;
  flex-direction: row;
  align-items: center;
  list-style: none;
}
li{
  margin: 0 16px;
}
li a{
  color: #ccc;
  font-size: 14px;
}
li a:hover{
  color: #fff;
}
</style>