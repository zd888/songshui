<template>
	<view class="page-box">
		<view class="centre">
			<image src="../static/images/empty.png" mode=""></image>
			<view class="tips">
				{{content}}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			content: {
				type: String,
				default: '暂无数据'
			}
		}
	}
</script>

<style lang="scss" scoped>
	
	.page-box {
		position: relative;
		left: 0;
		height: 100vh;
		background: #ffffff;
	}

	.centre {
		position: absolute;
		left: 0;
		top: 0;
		right: 0;
		bottom: 0;
		margin: auto;
		height: 0rpx;
		text-align: center;
		// padding: 200rpx auto;
		font-size: 32rpx;

		image {
			// width: 387rpx;
			// height: 341rpx;
			width: 340rpx;
			height: 270rpx;
			// margin-bottom: 20rpx;
			margin: 0 auto 20rpx;
			// border: 1px dotted #000000;
		}

		.tips {
			font-size: 32rpx;
			color: #2F3044;
			margin-top: 20rpx;
			font-weight: 700;
		}

		.btn {
			margin: 80rpx auto;
			width: 600rpx;
			border-radius: 32rpx;
			line-height: 90rpx;
			color: #ffffff;
			font-size: 34rpx;
			background: #5074FF;
		}
	}
</style>
