<template>
	<div class="navbar">
		<div class="nav" v-for="nav in navList" :class="{'active':nav.code==clicked}" @click="clickNav(nav)">{{nav.name}}</div>
	</div>
</template>

<script>
	export default {
		name: 'navbar',
		//接收从父组件传过来的数据
		props: {
			clicked: String
		},
		data:function(){
			return {
				navList:[
					{name:'正在热映',code:'in_theaters'},
					{name:'即将上映',code:'coming_soon'},
					{name:'top250',code:'top250'},
					{name:'口碑榜',code:'weekly'},
					{name:'北美票房榜',code:'us_box'},
					{name:'新片榜',code:'new_movies'}
				]
			}
		},
    mounted(){
      this.clickNav(this.navList[0]);
    },
		methods:{
			clickNav:function(nav){
				//this.clicked = nav.code;
				this.$emit('getClicked',nav.code,nav.name);
			}
		}
	}
</script>

<style>
	.navbar{
		display: flex;
		background-color: #FFFFFF;
	}
	.nav{
		flex: 1;
		text-align: center;
		padding-top: 1em;
		padding-bottom: 1em;
		border: 1px #DDDDDD solid;
	}
	.navbar .active{
		background-color: #dde7e8 !important;
	}
</style>
