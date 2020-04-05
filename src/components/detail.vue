<template>
	<div class="wrapper" ref="wrapper">
		<div class="content">
			<meta name="referrer" content="never">
			<ul v-show="!showDetailFlag">
				<li v-for="item in list" :id="item.id" @click="showDetail(item)">
					<div class="hang">
						<div class="cover" v-if="item&&item.images&&item.images.small"><img :src="item.images.small" /></div>
						<div class="flex">
							<a @click="detail(item)">{{item.title}}<span v-if="item.title!=item.original_title"> / {{item.original_title}}</span></a>
							<div>导演:<span v-for="director in item.directors"> {{director.name}}</span></div>
							<div>主演:<span v-for="cast in item.casts"> {{cast.name}}</span></div>
							<div>{{item.year}}<span v-for="genre in item.genres"> {{genre}}</span></div>
						</div>
					</div>
				</li>
			</ul>
			<detail :movieid="movieid" v-if="showDetailFlag" @backList="closeDetail"/>
		</div>
	</div>
</template>

<script>
import detail from './detail.vue'
import BScroll from 'better-scroll'
import $ from 'jquery'
export default {
	name: 'top250',
	components: {
		detail
	},
	data:function(){
		return {
			list:[],
			start:0,
			count:10,
			listscroll:null,
			showDetailFlag:false
		}
	},
	mounted(){
		this.list = [];
		this.getList();
	},
	methods: {
		getList:function(){
			var _this = this;
			if(_this.isLoaded){
				return;
			}
			//豆瓣接口apikey
			var apikey='0df993c66c0c636e29ecbb5344252a4a';
			var sendObj = {
				start:_this.start,
				count:_this.count,
				apikey:apikey
			};
			$.ajax({
				url:'https://api.douban.com/v2/movie/top250',
				type:'GET',
				data:sendObj,
				dataType:'json',
				timeout:8000,
				success:function(data){
					console.log(data);
					
					if(data&&data.subjects&&data.subjects.length>0){
						_this.list = _this.list.concat(data.subjects);
						_this.start+=_this.count;
						console.log(_this.start);
					}
					_this.$nextTick(() => {
						if(_this.listscroll){
							_this.listscroll.finishPullUp();
							_this.listscroll.refresh();
						}else{
							_this.listscroll = new BScroll(_this.$refs.wrapper, {
								//开启点击事件 默认为false
								click:true,
								snap:true,
								probeType:3,
								pullUpLoad: {
									threshold: 50 // 当上拉距离超过50px时触发 pullingUp 事件
								}
							})
							// 触发上拉加载的事件， 调用getList 请求数据
							_this.listscroll.on('pullingUp', () => {
								console.log('上拉加载')
								_this.getList();
							})
						}
					})
				},
				error:function(xhr, errorType, error,msg){
					alert(msg);
					_this.$nextTick(() => {
						_this.listscroll.refresh()
					})
				}
			})
		},
		showDetail:function(item){
			var _this = this;
			_this.movieid = item.id;
			_this.showDetailFlag = true;
			setTimeout(()=>{
				_this.listscroll.scrollTo(0, 0,0);
			},500);
		},
		closeDetail:function(flag){
			var _this = this;
			_this.showDetailFlag = false;
			setTimeout(()=>{
				let a = document.getElementById(''+_this.movieid);
				_this.listscroll.scrollToElement(a,0,0);
			},500);
		}
	}
}
</script>
	
<style>
	.wrapper{
		flex:1;
		flex-direction: column;
		position: relative;
		overflow: hidden;
	}
	.hang{
		width: 100%;
		height: auto;
		display: flex;
	}
	.cover,.cover img{
		width: 100px;
		height: auto;
	}
	ul,li{
		list-style: none;
		margin: 0;
		padding: 0;
	}
</style>
