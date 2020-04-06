<template>
	<div id="app">
		<!--
		<HelloWorld msg="Welcome to Your Vue.js App" v-if="flag2===1"/>
		-->
    <div class="top-nav">
      <div class="left-btn">
        <div v-show="showDetailFlag" @click="closeDetail"><i class="fa fa-chevron-left" aria-hidden="true"></i>返回列表</div>
      </div>
      <div class="title">{{title}}</div>
      <div class="right-btn">说明</div>
    </div>
		<movielist v-show="!showDetailFlag" ref="movielist" @showDetail="showDetailFromMovielist"/>
    <detail v-show="showDetailFlag" ref="detail"/>
		<navbar :clicked="clicked" @getClicked="getClickedFromNavbar"/>
    <router-view/>
	</div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import movielist from './components/movielist.vue'
import detail from './components/detail.vue'
import navbar from './components/navbar.vue'

export default {
  name: 'app',
  components: {
    navbar,
    movielist,
    detail
  },
  data:function(){
	  return {
		  clicked:'',
      title:'',
      old_title:'',
      showDetailFlag:false
	  }
  },
  mounted(){

  },
  methods:{
	  getClickedFromNavbar:function(clicked,clickedName){
		  this.clicked = clicked;
      this.title = clickedName;
      this.showDetailFlag = false;
		  this.$refs.movielist.init(this.clicked);
	  },
    showDetailFromMovielist:function(movieid,moviename){
      this.showDetailFlag = true;
      this.old_title = this.title;
      this.title = moviename;
      this.$refs.detail.init(movieid);
    },
    closeDetail:function(){
      this.showDetailFlag = false;
      this.title = this.old_title;
    }
  }
}
</script>

<style>
body{
	margin: 0;
	padding: 0;
}
#app {
  width: 100%;
  height: 100%;
  position: fixed;
  display: flex;
  flex-direction: column;
  z-index: 0;
}
/**
 * 去掉列表的默认样式
 */
ul,li{
  list-style: none;
  margin: 0;
  padding: 0;
}
/**
 * 滚动列表
 */
.wrapper{
  flex:1;
  position: relative;
  overflow: hidden;
}
/**
 * 边距
 */
.mt1{
  margin-top: 0.5em;
}

/**
 * 顶部的导航栏
 */
.top-nav{
  background-color: #FFFFFF;
  border: 1px #DDDDDD solid;
  padding-top: 0.5em;
  padding-bottom: 0.5em;
  width: 100%;
  display: flex;
}
.top-nav .left-btn{
  margin-left: 0.5em;
  width: 5em;
}
.top-nav .title{
  flex: 1;
  text-align: center;
}
.top-nav .right-btn{
  margin-right: 0.5em;
}
</style>
