<!-- 商品列表组件 <good-list :list="xx"></good-list> -->
<template>
	<view>
		<view class="flex flex-wrap margin-lr justify-between margin-top">
			<view class="listbox margin-bottom" v-for="(item,index) in List" :key='index'
				@tap="clickItem(item,index)">
				<image :src="item.homepageImg?item.homepageImg:'../../static/logo.png'"></image>
				<view class="padding-lr-sm padding-tb-sm">
					<view>{{item.serviceName}}</view>
					<view class="text-sm margin-tb-xs text-cut" style="color: #999999;width: 295upx;">{{item.myLevel}}</view>
					<view class="flex align-center">
						<view style="color: #FD3C44;font-size: 38upx;">{{isVip? item.memberMoney :item.money}}<text class="text-26">积分</text></view>
						<!-- <view style="color: #FD3C44;">+</view>
						<view style="color: #FD3C44;font-size: 38upx;"><text class="text-26">￥</text>29.0</view> -->
						<!-- <view class="labels">水贝抵29</view> -->
					</view>
					<view class="flex align-center justify-between margin-top-xs"
						style="color: #CCCCCC;font-size: 24upx;">
						<view style="text-decoration: line-through;"></view>
						<view class="butt">兑换</view>
					</view>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		name: 'GoodList',
		props: {
			List: {
				type: Array,
				default () {
					return []
				}
			},
			isVip:{
				type: Boolean,
				default: false
			}
		},
		methods: {
			clickItem(item, index) {
				this.$emit('click', {
					item: item,
					index: index,
				});
			}
		}
	}
</script>

<style lang="scss">
	.listbox {
		background: #FFFFFF;
		border-radius: 16upx;
	}

	.listbox image {
		width: 333upx;
		height: 333upx;
		background: #FFFFFF;

		// border-radius: 16upx;
	}

	.butt {
		background: #025EFD;
		color: #FFFFFF;
		padding: 10upx 25upx;
		border-radius: 50upx;
	}
</style>
