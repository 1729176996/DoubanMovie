<template>
	<div id="app">
		<!--
		<HelloWorld msg="Welcome to Your Vue.js App" v-if="flag2===1"/>
		-->
		<movielist ref="movielist"/>
		<navbar :clicked="clicked" @getClicked="getClickedFromNavbar"/>
	</div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import movielist from './components/movielist.vue'
import navbar from './components/navbar.vue'

export default {
  name: 'app',
  components: {
    navbar,
	movielist
  },
  data:function(){
	  return {
		  clicked:''
	  }
  },
  mounted(){
	this.clicked = 'top250';
  	this.$refs.movielist.init(this.clicked);
  },
  methods:{
	  getClickedFromNavbar:function(clicked){
		  this.clicked = clicked;
		  this.$refs.movielist.init(this.clicked);
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
</style>
