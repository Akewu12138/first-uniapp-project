<template>
	<view class="user">
		<view class="top">
			<image src="/static/images/history.png" mode=""></image>
			<view class="text">历史记录</view>
		</view>
		<view class="content">
			<view class="row" v-for="item in listArr">
				<newsbox :item="item" @click.native="goDetail(item)"></newsbox>
			</view>
		</view>
		<view class="nohistory" v-if="!listArr.length">
			<image src="/static/images/nohis.png" mode="widthFix"></image>
			<view class="text">暂无浏览记录</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				listArr:[]
			}
		},
		onShow() {
			this.getData()
		},
		methods: {
			//跳转详情页面
			goDetail(item){
				uni.navigateTo({
					url:`/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				})
			},
			
			//获取缓存浏览记录
			getData(){
				let hisArr = uni.getStorageSync("historyArr") || []
				this.listArr = hisArr
				console.log(this.listArr)
			}
		}
	}
</script>

<style lang="scss">
.user{
	.top{
		padding: 50rpx 0;
		background: #F8F8F8;
		color:#666;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		image{
			width: 250rpx;
			height: 250rpx;
		}
		.text{
			font-size: 38rpx;
			padding-top: 20rpx;
		}
	}
	
	// 内容区样式
	.content{
		padding: 30rpx;
		.row{
			border-bottom: 1rpx dotted #efefef;
			padding: 15rpx 0;
		}
	}
	
	.nohistory{
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		image{
			width: 450rpx;
		}
		.text{	
			font-size: 26rpx;
			color: #888;
		}
	}
}

</style>
