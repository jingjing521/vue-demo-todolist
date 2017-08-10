<template>
  <div id="app">
     <h3>{{title}}</h3>
     <input @keyup.enter="addTask"  class="todo" type="text" v-model="task.content" placeholder="编写任务">
     <div class="list">
      <div class="unit" v-for="(item,index) in list">
          <input  @click="changeState(index)" :checked="item.finished" type="checkbox">
          <span :class="{'finish':item.finished}">{{item.content}}</span>
          <button @click="removeTask(index)" class="del">删除</button>
      </div>
   </div>
  </div>
</template>
<script>
import Store from './store'
export default {
  data:function(){
    return {
      title:'this is a todo list',
      //默认
      task:{
        content:'',//内容为空
        finished:false,//未完成
        deleted:false//未删除
      },
      //任务列表
      list:Store.fetch()
    }
  },
  watch:{
    list: {
        handler: function (item) {Store.save(item)},
        deep: true
      }
  },
  methods:{
    //添加任务
        addTask:function(){
              //将task存入list数组
            this.list.push(this.task);
              //存入list[]后，重置task
              this.task = {
                  content:'',//内容为空
              finished:false,//未完成
              deleted:false//未删除
          }
            },
            changeState:function(index){
          let curState =  this.list[index].finished;
          this.list[index].finished = !curState;
      },
      removeTask:function(index){
          //使用splice操作删除数组指定项
          this.list.splice(index,1);
      }
  }
}
</script>
<style>
  .todo{
      display:block ;
      width:80%;
      height: 35px;
      line-height: 35px;
      margin: 30px auto;
      box-sizing: border-box;
      padding-left: 4px;
      border-radius: 4px;
      border:1px solid #ccc;
      outline: 0;
      box-shadow: 0 0 5px #ccc;
   }
   .list{
      margin: 0 auto;
      width:80%;
   }
   .unit{
      position: relative;
      padding: 10px 0;
      border-bottom: 1px solid #efefef;
      text-align: left;
   }
   .unit:last-child{
      border-bottom: 0;
   }
   .finish{
      text-decoration: line-through;
      color: #ccc;
   }
    .del{
      background: red;
      text-decoration: none;
      position: absolute;
      right:0;
      font-size: 12px;
      border: 0;
      outline: 0;
      padding: 2px 5px;
      border-radius: 5px;
      color: #fff;
   }
    .empty{
      font-size: 13px;
      color: #ccc;
      text-align: center;
      padding: 10px 0;
   }
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
