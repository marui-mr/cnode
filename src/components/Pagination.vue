<template>
  <div class="pagination">
    <button @click="changeBtn">首页</button>
    <button @click="changeBtn">上一页</button>
    <button v-if="jduge" class="pageBtn">......</button>
    <button v-for="(btn,index) in buttons" :key="index" @click="changeBtn(btn)"
            :class="[{currentPage:btn===currentPage},'pageBtn']">
      {{btn}}
    </button>
    <button @click="changeBtn">下一页</button>
  </div>
</template>

<script>
import $ from 'jquery'
export default {
  name: "Pagination",
  data(){
    return {
      buttons:[1,2,3,4,5,'......'],
      currentPage:1,
      jduge:false
    }
  },
  methods:{
    changeBtn(page){
      // 点击上一页、下一页、首页
      if (typeof page !== 'number'){
        switch(page.target.innerText){
          case '上一页':
            $('button.currentPage').prev().click()
            break
          case '下一页':
            $('button.currentPage').next().click()
            break
          case '首页':
            this.buttons = [1,2,3,4,5,'......']
            this.changeBtn(1)
            break
          default:
            break
        }
        return
      }
      this.currentPage = page
      if (page>4){
        this.jduge =true
      }else{
        this.jduge = false
      }
      if (page===this.buttons[4]){
        this.buttons.shift()
        this.buttons.splice(4,0,this.buttons[3]+1)
      }else if (page===this.buttons[0] && page !== 1){
        this.buttons.splice(4,1)
        this.buttons.unshift(this.buttons[0]-1)
      }
      this.$emit('handleList',this.currentPage)
    }
  }
}
</script>

<style scoped>
.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: white;
  padding: 10px;
  border-radius: 5px;
  width: 100%;
}
button {
  background-color: #fff;
  border: 1px solid #ddd;
  color: #778087;
  border-radius: 3px;
  outline: none;
  margin-right: 5px;
  cursor: pointer;
  width: 56px;
  height: 29px;
}
.pageBtn {
  width: 45px;
}
.currentPage {
  color: white;
  background-color: #1f1b1b;
}
</style>