<template>
	<view class="home">
		<!-- 新闻选择栏 -->
		<scroll-view scroll-x class="navScroll">
			<view class="item" :class="index==navIndex ? 'active' : ''" 
			v-for="(item,index) in navArr"  @click="clickNav(index,item.id)" 
			:key="item.id">{{item.classname}}</view>
		</scroll-view>
		
		<!-- 左图右文内容去 -->
		<view class="content">
			<view class="row" v-for="item in newsArr" :key="item.id">
				<newsbox :item="item" @click.native="goDetail(item)"></newsbox>
			</view>
		</view>
		
		<!-- //无数据显示 -->
		<view class="nodata" v-if="!newsArr.length">
			<image src="/static/images/nodata.png" mode="widthFix"></image>
		</view>
		
		<!-- 加载显示 -->
		<view class="loading" v-if="newsArr.length">
			<view v-if="loading==1">数据加载中……</view>
			<view v-if="loading==2">没有更多数据……</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navIndex: 0,
				navArr: [],
				newsArr: [],
				currentPage: 1,
				loading: 0,
			}
		},
		onLoad() {
			this.getNavData();
			this.getNewsData();
		},
		onReachBottom() {
			if(this.loading==2){
				return;
			} 
			this.currentPage++;
			this.loading=1;
			this.getNewsData(); 
		},
		
		methods: {
			//点击导航
			clickNav(index,id){
				this.navIndex = index;
				this.currentPage = 1;
				this.loading = 0;
				this.newsArr = []
				this.getNewsData(id);
			},
			
			//跳转详情页面
			goDetail(item){
				uni.navigateTo({
					url:`/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				})
			},
			
			//获取导航数据
			getNavData(){
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/navlist.php",
					success: res => {
						console.log(res)
						this.navArr = res.data
					}
				})
			},
			
			//获取新闻列表数据
			getNewsData(id=50){
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/newslist.php",
					data:{
						cid:id,
						num: 10,
						page: this.currentPage
					},
					success: res => {	
						console.log(res)
						if(res.data.length==0){
							this.loading = 2
						}
						this.newsArr = [...this.newsArr,...res.data]
					}
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	// 选择栏样式
	.navScroll{
		height: 100rpx;
		background: #F7F8FA;
		white-space: nowrap;
		position: fixed;
		top: var(--window-top);
		left: 0;
		z-index: 10;
		/deep/ ::-webkit-scrollbar{
			width: 40rpx !important;
			height: 1rpx !important;
			overflow: auto !important;
			background: transparent !important;
			-webkit-appearance: auto !important;
			display: block;
		}
		.item{
			font-size: 40rpx;
			display: inline-block;
			line-height: 100rpx;
			padding: 0 30rpx;
			color: #333;
			&.active{
				color: #31c27c;
			}
		}
		
	}
	
	// 内容区样式
	.content{
		margin-top: 60rpx;
		padding: 30rpx;
		.row{
			border-bottom: 1rpx dotted #efefef;
			padding: 15rpx 0;
		}
	}
	
	.nodata{
		display: flex;
		justify-content: center;
		image{
			width: 360rpx;
		}
	}
	
	.loading{
		text-align: center;
		color: #999;
	}
	
</style>
