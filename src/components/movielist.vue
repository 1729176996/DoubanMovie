<template>
	<div class="wrapper" ref="wrapper">
		<div class="content">
			<meta name="referrer" content="never">
			<ul>
				<li v-for="item in list" :id="item.id">
					<div class="hang" v-if="item.subject">
						<div class="cover" v-if="item&&item.subject&&item.subject.images&&item.subject.images.small"><img :src="item.subject.images.small" /></div>
						<div class="flex">
							<a>{{item.subject.title}}<span v-if="item.subject.title!=item.subject.original_title"> / {{item.subject.original_title}}</span></a>
							<div>导演:<span v-for="director in item.subject.directors"> {{director.name}}</span></div>
							<div>主演:<span v-for="cast in item.subject.casts"> {{cast.name}}</span></div>
							<div>{{item.year}}<span v-for="genre in item.subject.genres"> {{genre}}</span></div>
						</div>
					</div>
					<div class="hang" v-if="!item.subject">
						<div class="cover" v-if="item&&item.images&&item.images.small"><img :src="item.images.small" /></div>
						<div class="flex">
							<a>{{item.title}}<span v-if="item.title!=item.original_title"> / {{item.original_title}}</span></a>
							<div>导演:<span v-for="director in item.directors"> {{director.name}}</span></div>
							<div>主演:<span v-for="cast in item.casts"> {{cast.name}}</span></div>
							<div>{{item.year}}<span v-for="genre in item.genres"> {{genre}}</span></div>
						</div>
					</div>
				</li>
			</ul>
		</div>
	</div>
</template>

<script>
import BScroll from 'better-scroll'
import $ from 'jquery'
export default {
	name: 'movielist',
	data:function(){
		return {
			list:[],//列表
			start:0,//查询开始位置
			count:10,//每次查询个数
			listscroll:null,
			type:''//查询类别
		}
	},
	mounted(){

	},
	methods: {
		init:function(type){
			this.type = type;
			this.getList(true);
		},
		getList:function(initFlag){
			var _this = this;
			var start = _this.start;
			if(initFlag){
				start = 0;
				_this.list = [];
			}else{
				if(_this.type=='weekly'||_this.type=='us_box'||_this.type=='new_movies'){
					_this.listscroll.finishPullUp();
					_this.listscroll.refresh();
					return;
				}
			}
			//豆瓣接口apikey
			var apikey='0df993c66c0c636e29ecbb5344252a4a';
			var sendObj = {
				start:start,
				count:_this.count,
				apikey:apikey
			};
			$.ajax({
				url:'https://api.douban.com/v2/movie/'+_this.type,
				type:'GET',
				data:sendObj,
				dataType: "jsonp",
        jsonp: "callback",//传递给请求处理程序或页面的，用以获得jsonp回调函数名的参数名(一般默认为:callback)
				timeout:8000,
				success:function(data){
					console.log(data);

					if(data&&data.subjects&&data.subjects.length>0){
						_this.list = _this.list.concat(data.subjects);
						_this.start= start + _this.count;
					}
					_this.$nextTick(() => {
						if(initFlag){
							if(_this.listscroll){
								_this.listscroll.destroy();
							}
							_this.listscroll = new BScroll(_this.$refs.wrapper, {
								//开启点击事件 默认为false
								click:true,
								pullUpLoad: {
									threshold: 50 // 当上拉距离超过50px时触发 pullingUp 事件
								}
							})
							// 触发上拉加载的事件， 调用getList 请求数据
							_this.listscroll.on('pullingUp', () => {
								console.log('上拉加载')
								_this.getList(false);
							})
						}else{
							_this.listscroll.finishPullUp();
							_this.listscroll.refresh();
						}
					})
				},
				error:function(xhr, errorType, error,msg){
					console.log(msg);
					_this.$nextTick(() => {
						if(_this.listscroll){
							_this.listscroll.finishPullUp();
							_this.listscroll.refresh();
						}
					})
				}
			})
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
