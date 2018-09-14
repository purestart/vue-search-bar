<template>
  <div class="home-container">
    <div class="Header">搜索工具条</div>
    <div class="search-box">

        <search-bar :onChange="onChange" :onSearch="onSearch">
          <div class="hot-wrapper">
            <div class="tip">热门搜索</div>
            <div class="label-wraper">
              <div class="label">SearchBar</div>
              <div class="label">前端Vue学习</div>
              <div class="label">redis缓存</div>
              <div class="label">组件开发</div>
              <div class="label">react组件</div>
            </div>
            <div class="search-list">
                <div :key="index" v-for="(item,index) in reslist" class="list-item">
                    <div class="left-item">
                      <i class="iconfont icon-search1187938easyiconnet"></i>
                      <span>{{item}}</span>
                    </div>
                    <div>
                    </div>
                </div>
            </div>
          </div>
        </search-bar>

      </div>
    <div class="item-wrapper">
       <div class="item-image">
         <!-- <ap-image :showClose="false" :thumb="imgUrl" :radius="3" :full="imgUrl" :maxWidth="300"></ap-image> -->
         <img style="width:100%;" :src="imgUrl" />
       </div>
       <div class="item-right">
          <div style="font-size:3.6vw;">新闻标题图片预览组件</div>
          <div style="font-size:3.4vw;color:#777;">描述</div>
       </div>
    </div>

  </div>
</template>

<script>


export default {
  name: 'el-index',
  components:{
  },
  data(){
    return{
          imgUrl:"http://data618.oss-cn-qingdao.aliyuncs.com/ys/3524/img/b.jpg",
          reslist:["如何创建一个Vue组件","Vue的生命周期",'Vue路由'],
    }
  },
  mounted(){

  },
  methods:{
    onSearch(value){

    },
    onChange(value){

    }
  }
};
</script>

<style lang="scss" scoped>

.test{
  display: flex;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: -moz-box;
  display:-webkit-box;

  justify-content:space-around;
  box-pack:space-around;
  -webkit--moz-box-pack:space-around;
  -moz-box-pack:space-around;
  -webkit-justify-content:space-around;

  flex:1;
  -webkit-flex:1;
  -moz-box-flex:1;
  -webkit-box-flex:1;
  box-flex:1;


  flex-wrap:wrap;
  -moz-flex-wrap:wrap;
  -webkit-box-lines:multiple;
  -webkit-flex-wrap:wrap;

}

.tab {
  text-align: center;
  padding: 10px;
  margin: 5px 0;
  background-color: #F2F2F2;
  a {
    color: #7e8c8d;
  }
}

  .home-container{
    width: 100%;

    .Header{
      background:linear-gradient(90deg, rgb(231, 86, 142), #ff696b);
      padding: 4vw;
      color: #fff;
      text-align: center;
    }
    .item-wrapper{
      display: flex;
      padding: 4vw;
    }
    .item-image{  
      width: 25vw;
    }
    .item-right{
      flex: 1;
      margin-left: 3vw;
      display: flex;
      justify-content: space-between;
      flex-direction: column;
      padding: 1vw 0;
      
    }
  }


.search-box{

  .hot-wrapper{
    text-align: left;
    .tip{
      padding: 4vw;
      text-align: left;
      font-size: 3.0vw;
      color: #999999;
    }
    .label-wraper{
      display: flex;
      flex-wrap: wrap;
      padding:0 4vw;
      border-bottom:1px solid #e4e4e4;

      .label{
        padding: 0.8vw 2vw;
        border-radius: 1vw;
        background-color: #efefef;
        font-size: 3.4vw;
        margin-bottom: 4vw;
        margin-right: 2vw;
        color: #666;
      }
    }
    .search-list{
      color: #555;
      .list-item{
        display: flex;
        align-items: center;
        padding: 4vw;
        border-bottom: 1px solid #e4e4e4;
        font-size: 3.6vw;
        &:active{
          background-color: #efeff4;
        }
        .list-item{
          display: flex;
          align-items: center;
        }
      }
    }
  }

}


</style>
