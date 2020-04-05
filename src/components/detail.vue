<template>
	<div class="wrapper" ref="wrapper">
		<div class="content">

      <div class="detail-cover" v-if="detailObj&&detailObj.images&&detailObj.images.large">
      	<img :src="detailObj.images.large" />
      </div>
      <div class="mt1">
      	<b>{{detailObj.title}} <span class="c-grey"> ({{detailObj.year}})</span></b>
      </div>
      <div class="mt1" v-if="detailObj.title!=detailObj.original_title">
      	<b>{{detailObj.original_title}}</b>
      </div>
      <div class="mt1">导演: <span v-for="director,index in detailObj.directors"><a v-if="index!=0"> / </a>{{director.name}}</span></div>
      <div class="mt1">编剧: <span v-for="writer,index in detailObj.writers"><a v-if="index!=0"> / </a>{{writer.name}}</span></div>
      <div class="mt1">主演: <span v-for="cast,index in detailObj.casts"><a v-if="index!=0"> / </a>{{cast.name}}</span></div>
      <div class="mt1">类型: <span v-for="genre,index in detailObj.genres"><a v-if="index!=0"> / </a>{{genre}}</span></div>
      <div class="mt1">制片国家/地区: <span v-for="country,index in detailObj.countries"><a v-if="index!=0"> / </a>{{country}}</span></div>
      <div class="mt1">语言: <span v-for="language,index in detailObj.languages"><a v-if="index!=0"> / </a>{{language}}</span></div>
      <div class="mt1">上映日期: <span v-for="pubdate,index in detailObj.pubdates"><a v-if="index!=0"> / </a>{{pubdate}}</span></div>
      <div class="mt1">片长: <span v-for="duration,index in detailObj.durations"><a v-if="index!=0"> / </a>{{duration}}</span></div>
      <div class="mt1">又名: <span v-for="ak,index in detailObj.aka"><a v-if="index!=0"> / </a>{{ak}}</span></div>
      <div class="mt1 watch-list" v-if="detailObj.videos&&detailObj.videos.length>0">
      	<div class="list-item" style="color: darkorange;">观看:</div>
      	<div class="list-item" v-for="video in detailObj.videos" @click="view(video.sample_link)">
      		<div style="padding-left: 4em;flex: 1;color:chartreuse;">{{video.source.name}}</div>
      		<div style="padding-right: 1em;">{{video.need_pay?'VIP免费观看':'免费观看'}}</div>
      	</div>
      </div>

		</div>
	</div>
</template>

<script>
import BScroll from 'better-scroll'
import $ from 'jquery'
export default {
	name: 'detail',
	data:function(){
		return {
			detailObj:{},
			detailscroll:null
		}
	},
	mounted(){

	},
	methods: {
		init:function(movieid){
			var _this = this;
			//豆瓣接口apikey
			var apikey='0df993c66c0c636e29ecbb5344252a4a';
			var sendObj = {
				apikey:apikey
			};
			$.ajax({
				url:'https://api.douban.com/v2/movie/subject/'+movieid,
				type:'GET',
				data:sendObj,
				dataType: "jsonp",
		    jsonp: "callback",//传递给请求处理程序或页面的，用以获得jsonp回调函数名的参数名(一般默认为:callback)
				timeout:8000,
				success:function(data){
					console.log(data);

					_this.detailObj = data?data:{};

					_this.$nextTick(() => {
						if(_this.detailscroll){
							_this.detailscroll.destroy();
						}
						_this.detailscroll = new BScroll(_this.$refs.wrapper, {
							//开启点击事件 默认为false
							click:true
						})
					})
				},
				error:function(xhr, errorType, error,msg){
					console.log(msg);
					_this.$nextTick(() => {
						if(_this.detailscroll){
							_this.detailscroll.finishPullUp();
							_this.detailscroll.refresh();
						}
					})
				}
			})
		},
    view:function(href){
      window.open(href,"_blank");
    }
	}
}
</script>

<style>
.detail-cover,.detail-cover img{
  width: 100%;
  height: auto;
}

.watch-list{

}
.watch-list .list-item{
  width: 100%;
  height: 40px;
  display: flex;
  line-height: 40px;
  border: 1px #e1e1e2 solid;
}
</style>
