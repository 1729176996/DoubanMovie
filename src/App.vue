<template>
	<div id="app">
		<!--
		<HelloWorld msg="Welcome to Your Vue.js App" v-if="flag2===1"/>
		-->
		<movielist v-show="!showDetailFlag" ref="movielist" @showDetail="showDetailFromMovielist"/>
    <div class="detail-nav" v-show="showDetailFlag" @click="closeDetail">返回列表</div>
    <detail v-show="showDetailFlag" ref="detail"/>
		<navbar :clicked="clicked" @getClicked="getClickedFromNavbar"/>
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
      showDetailFlag:false
	  }
  },
  mounted(){
	this.clicked = 'top250';
  	this.$refs.movielist.init(this.clicked);
  },
  methods:{
	  getClickedFromNavbar:function(clicked){
		  this.clicked = clicked;
      this.showDetailFlag = false;
		  this.$refs.movielist.init(this.clicked);
	  },
    showDetailFromMovielist:function(movieid){
      this.showDetailFlag = true;
      this.$refs.detail.init(movieid);
    },
    closeDetail:function(){
      this.showDetailFlag = false;
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

.detail-nav{
	background-color: #FFFFFF;
	border: 1px #DDDDDD solid;
  padding-top: 0.5em;
  padding-bottom: 0.5em;
  padding-left: 0.5em;
}
.detail-nav::before{
  content: "< ";
}
</style>
