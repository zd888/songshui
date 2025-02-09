<template>
	<view>
		<view class=" u-skeleton-fillet" style="position: relative;">
			<view class="bg u-flex" @click="getAddressList()" style="padding: 25rpx 25rpx 20rpx 25rpx; ">
				<view class="u-flex-1 text-white margin-left-xs">
					<view class="padding-tb" v-if="detailaddress ==''">
						<view class="text-lg text-bold margin-bottom-xs">服务地址</view>
						<view style="color: #999999;">请选择服务地址</view>
					</view>
					<view v-else>
						<view class="u-font-16  text-bold">{{province}}{{city}}{{district}}{{detailaddress}}</view>
						<view class="u-font-14 margin-top-xs u-tips-color flex justify-between">
							<view style="color: #333333;">{{name}}<text class="margin-left-sm">{{mobile}}</text>
							</view>
						</view>
					</view>
				</view>
				<u-icon name="arrow-right" color="#999999"></u-icon>
			</view>
			<image src="../../static/images/index/line.png" style="width: 100%;height: 6upx;position: absolute;">
			</image>
		</view>

		<view class="padding-tb">
			<view class=" bg  u-skeleton-fillet  " style="padding: 25rpx;" v-for="(item,index) in order" :key="index">
				<view class="flex" style="position: relative;">
					<image :src="item.orderTaking.homepageImg?item.orderTaking.homepageImg:'../../../static/logo.png'"
						style="width: 160upx;height: 160upx;" mode="aspectFill"></image>
					<view class="u-flex-1 text-white margin-left-xs">
						<view class="  u-line-1" style="width: 480rpx;">{{item.orderTaking.serviceName}}</view>
						<view class="flex align-center margin-tb-sm text-sm" style="color: #999999;">
							<view v-if="item.detailJson">{{item.detailJson}}</view>
							<view class="margin-left-xs">数量 x{{item.orderNumber}}</view>
						</view>
						<view style="color: #FF2D01;font-size: 32upx;font-weight: bold;">
							<text class="text-sm">￥</text>{{item.money}}
						</view>
					</view>
				</view>
				<view class=" bg margin-top  u-skeleton-fillet">
					<view class="flex align-center justify-between ">
						<view class="text-30" style="color: #1E1F31;width: 240upx;">配送时间</view>
						<view class="flex justify-between " @click="openTiem(index)">
							<view>{{item.startTime}}</view>
						</view>
					</view>
				</view>
				<view class="flex align-center justify-between margin-top">
					<view class="text-30" style="color: #1E1F31;width: 240upx;">附近站点</view>
					<view class="flex justify-between">
						<view>{{laundryName?laundryName:'请选择附近站点'}}</view>
					</view>
				</view>
				<view class="flex align-center justify-between  margin-top" v-if="youhuiList && youhuiList.length!=0">
					<view class="text-30" style="color: #1E1F31;width: 240upx;">优惠劵</view>
					<view class="flex align-center" @click="openYouhui(item,index)">
						<view>{{-item.couponName?-item.couponName:'立即使用'}}</view>
						<image src="../../static/images/my/right.png"
							style="width: 12rpx;height: 24rpx;margin-left: 10rpx;"></image>
					</view>
				</view>
			</view>
			<!-- <view class="bg  padding">
				<view class="flex align-center justify-between  " v-if="youhuiList.length!=0">
					<view class="text-30" style="color: #1E1F31;width: 240upx;">优惠劵</view>
					<view class="flex align-center" @click="showYouhuijuan = true">
						<view>{{-couponName?-couponName:'立即使用'}}</view>
						<image src="../../static/images/my/right.png"
							style="width: 12rpx;height: 24rpx;margin-left: 10rpx;"></image>
					</view>
				</view>
			</view> -->

			<!-- <view class="padding bg margin-top  u-skeleton-fillet">
				<view class="text-30" style="color: #1E1F31;width: 240upx;">使用积分</view>
				<view class="flex align-center justify-between margin-top-sm">
					<view class="tetx-sm" style="color:#333333;">当前剩余积分30，可使用30积分优惠10元</view>
					<view class="text-right">
						<u-checkbox-group @change="checkboxGroupChange">
							<u-checkbox v-model="check" name='0' shape="circle" @change="checkboxChange"></u-checkbox>
						</u-checkbox-group>
					</view>
				</view>
			</view> -->

			<view class="text-white flex justify-between cu-bar foot bg ">
				<view class="flex align-center padding-lr">
					<view class="margin-right-xs">共<text class="text-lg text-bold "
							v-if="order">{{order.length}}</text>件</view>
					合计：<view style="font-size: 38upx;margin-top: 8upx;">{{totalMoney}}
						<image src="../../static/images/my/shuibei.png"
							style="width: 28rpx;height: 40rpx;margin: 0rpx 5rpx;"></image>
					</view>
				</view>
				<view class="shopcart" @click="Orderpay()">立即支付
					<!-- <u-button :custom-style="customStyle" @click="Orderpay()" shape="circle" ripple="false">立即支付</u-button> -->
				</view>
			</view>
		</view>
		<u-picker v-model="show" mode="time" :params="params" @confirm="statusChange"></u-picker>

		<!-- 支付弹框 -->
		<u-popup v-model="showpay" mode="bottom" :closeable="true" border-radius="14" close-icon="close-circle"
			close-icon-color="#ffffff" close-icon-size="48">
			<view class="popup_pay">
				<view class="text-bold padding-top margin-lr" style="font-size: 38upx;">支付方式</view>
				<view class="margin-top margin-lr margin-bottom-sm">
					<view class="flex align-center justify-between padding-tb-sm" style="height: 100upx;"
						v-for="(item,index) in openLists" :key='index'>
						<image :src="item.image" style="width: 50upx;height: 50upx;">
						</image>
						<view class="text-white flex align-end" style="width: 70%;">
							<view class="text-lg text-bold margin-right-xs">{{item.text}}</view>
							<view v-if="item.tit" class="text-sm">{{item.tit}}<text style="color: #2087fe;"
									@click="chongzhi">充值</text></view>
						</view>
						<radio-group name="openWay" style="margin-left: 45upx;" @tap.stop='selectWay(item)'>
							<label class="tui-radio">
								<radio :checked="openWay === item.id ? true : false" color="#025EFD" />
							</label>
						</radio-group>
					</view>
					<view style="display: flex;height: 100upx;align-items: center;padding: 20upx 0rpx;"
						v-if="money!=0&&isShuibei!='否'" v-for="(item,index) in openListl" :key='index'>
						<view style="display: flex;width:80%;align-items: center;">
							<image src="../../static/images/lingqian.png"
								style="width: 55upx;height: 55upx;border-radius: 50upx;"></image>
							<view class="text-lg text-bold" style="font-size: 30upx;margin-left: 30upx;width: 70%;">
								{{item.name}}
								<!-- <text style="font-size: 26rpx;margin-left:15rpx;">剩余{{water}}张</text> -->
							</view>
						</view>
						<view style="width: 20%;display: flex;justify-content: flex-end;">
							<radio-group name="openWay" style="margin-left: 20upx;" @tap='selectWay(item)'>
								<label class="tui-radio">
									<radio color="#1789FD" :checked="openWay === item.id ? true : false" />
								</label>
							</radio-group>
						</view>
					</view>
					<!-- <view style="display: flex;height: 100upx;align-items: center;padding: 20upx 0rpx;"
						v-if="waterList.length!=0&&!couponId" v-for="(item,index) in openList" :key='index'>
						<view style="display: flex;width:80%;align-items: center;">
							<image src="../../static/images/shuipiao.png"
								style="width: 55upx;height: 55upx;border-radius: 50upx;"></image>
							<view class="text-lg text-bold" style="font-size: 30upx;margin-left: 30upx;width: 70%;">
								{{item.name}}
								<text style="font-size: 26rpx;margin-left:15rpx;">剩余{{water}}张</text>
							</view>
						</view>
						<view style="width: 20%;display: flex;justify-content: flex-end;">
							<radio-group name="openWay" style="margin-left: 20upx;" @tap='selectWay(item)'>
								<label class="tui-radio">
									<radio color="#1789FD" :checked="openWay === item.id ? true : false" />
								</label>
							</radio-group>
						</view>
					</view> -->
				</view>
				<view class="pay_btn" @click="pay()">确认支付</view>
			</view>
		</u-popup>

		<u-popup v-model="showYouhuijuan" mode="bottom" border-radius="14" height="900rpx" :closeable="true">
			<view style="padding-bottom: 30rpx;">
				<view
					style="width: 100%;text-align: center;font-size:38rpx;font-weight: bold;padding-top:15rpx;padding-bottom:40rpx;">
					优惠劵
				</view>
				<view class="margin-right text-right" @click="qingkong">
					<view class="noyouhui">不使用优惠劵</view>
				</view>
				<view class="listbox" v-for="(item,ind) in youhuiList" :key="ind">
					<view class="flex align-start justify-between padding-lr">
						<view>
							<view style="color: #000000;font-size: 30upx;">{{item.couponName}}</view>
							<view style="color: #999999;font-size: 24upx;margin-top: 10upx;" v-if="item.expirationTime">
								有效期至{{item.expirationTime}}
							</view>
							<view style="color: #999999;font-size: 24upx;margin-top: 10upx;" v-else>永久有效
							</view>
						</view>
						<view style="color: #FD3C44;font-size: 30upx;">¥<text
								style="font-size: 48upx;font-weight: bold;">{{item.money}}</text></view>
					</view>
					<view style="width: 100%;border-top:1rpx dashed #E6E6E6;margin: 30upx 0upx;"></view>
					<view class="flex align-center justify-between padding-lr">
						<view v-if="item.minMoney">满{{item.minMoney}}元可用</view>
						<view v-else>无门槛使用</view>
						<view class="btn" @click="youhuiPay(item)">立即使用</view>
					</view>
				</view>
			</view>
		</u-popup>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				detailaddress: '',
				name: '',
				mobile: '',
				province: '',
				city: '',
				district: '',
				orderList: [],
				ordersIds: [],
				order: [],

				show: false,
				params: {
					year: true,
					month: true,
					day: true,
					hour: true,
					minute: true,
					second: false,
					timestamp: true
				},
				startTime: '立即配送',

				showpay: false,
				openLists: [],
				// openList: [{
				// 	id: 4,
				// 	name: '水票'
				// }],
				openListl: [{
					image: '../../../static/images/lingqian.png',
					name: '水贝',
					id: 3
				}],
				openWay: 1,
				timeIndex: '',
				formData: [],
				totalMoney: 0,

				water: '',
				waterList: [],
				roleId: '',

				showYouhuijuan: false,
				youhuiList: [],
				couponId: '',
				couponName: '',
				couponMoney: 0,
				youhuiMoney: 0,
				page: 1,
				limit: 10,

				youhuiIndex: 0,
				price: 0,
				money: 0,

				laundryName: '',
				laundryId: '',
				id: [],
				isShuibei: '否',
				bucket: 0
			}
		},
		onLoad(option) {
			this.totalMoney = option.totalMoney
			this.orderList = this.$queue.getData('orderList');
			// let arr = []
			// this.orderList.forEach(d=>{
			// 	let data = {}
			// 	data =d.ordersId
			// 	arr.push(data)
			// })
			// this.id = arr
			// console.log(JSON.stringify(this.orderList[0] ))
			this.$queue.remove('orderList');

			// this.getWaterlist()
			this.getMyList();
			this.getMyMoney()
			
			if (this.orderList) {
				let orderList = this.orderList;
				for (var i = 0; i < orderList.length; i++) {
					// console.error(orderList[i].ordersId) 
					this.ordersIds.push(orderList[i].ordersId)
					this.getDet()
					// this.getzhandian(orderList[i].ordersId);
				}
			}

			// #ifdef APP-PLUS
			this.openLists = [{
				image: '../../static/images/zhifubao.png',
				text: '支付宝',
				id: 1
			}, {
				image: '../../static/images/icon_weixin.png',
				text: '微信',
				id: 2
			}];
			this.openWay = 1;
			// #endif

			// #ifdef MP-WEIXIN
			this.openLists = [{
				image: '../../static/images/icon_weixin.png',
				text: '微信',
				id: 2
			}]
			this.openWay = 2;
			// #endif

			// #ifdef H5
			this.openLists = [{
				image: '../../static/images/zhifubao.png',
				text: '支付宝',
				id: 1
			}];
			this.openWay = 1;
			// #endif
		},
		onShow() {
			this.getUserInfo()
			this.isShuibei = this.$queue.getData('isShuibei')
			this.addressId = this.$queue.getData('EditAddress');
			if (this.addressId) {
				this.getAddressList(this.addressId);
			} else {
				this.addressMy()
			}
		},
		methods: {
			getzhandian(id) {
				// console.log(JSON.stringify(this.id))
				this.$Request.get("/app/laundry/selectLaundryByDistance", {
					latitude: this.latitude,
					longitude: this.longitude,
					goodsId: id
				}).then(res => {
					if (res.status == 0) {
						// this.laundry = res.data
						if (res.data && res.data.laundry_id) {
							let num = parseInt(res.data.distance).toFixed(2)
							console.log(num <= res.data.max_scope)
							if (num <= res.data.max_scope) {
								this.laundryId = res.data.laundry_id;
								this.laundryName =  res.data.laundry_name;
								// this.orderList.forEach((d, index) => {
								// 	if (d.orderTakingId == id) {
								// 		// this.order[index].laundryId = res.data.laundry_id;
								// 		// this.order[index].laundryName = res.data.laundry_name;
								// 	}
								// })
							} else {
								this.laundryId = ''
								this.laundryName = ''
								// this.orderList.forEach((d, index) => {
								// 	if (d.orderTakingId == id) {
								// 		this.order[index].laundryId = ''
								// 		this.order[index].laundryName = ''
								// 	}
								// })
								this.$queue.showToast('当前站点无法下单,请切换地址！');
							}
						} else {
							this.$queue.showToast('当前站点无法下单,请切换地址！');
							this.laundryName = '';
							this.laundryId = '';
						}
					} else {
						uni.showToast({
							title: res.msg,
							duration: 1000,
							icon: 'none'
						});
					}
				});
			},
			//我的余额
			getMyMoney() {
				this.$Request.get("/app/userMoney/selectMyMoney").then(res => {
					if (res.code == 0) {
						this.money = res.data.money
					}
				})
			},
			openYouhui(e, index) {
				this.showYouhuijuan = true
				this.youhuiIndex = index
				this.price = e.payMoney

			},
			qingkong() {
				// this.couponId = ''
				// this.couponName = ''
				this.order[this.youhuiIndex].couponId = ''
				this.order[this.youhuiIndex].couponName = ''
				let totalprce = this.totalMoney

				this.totalMoney = parseFloat(Number(totalprce) + Number(this.order[this.youhuiIndex].couponMoney)).toFixed(
					2)
				this.showYouhuijuan = false
			},
			youhuiPay(e) {
				// let totalprce = this.totalMoney
				// this.totalMoney = parseFloat(Number(totalprce) + Number(this.couponMoney)).toFixed(0)
				let price = this.totalMoney
				// console.log(this.order[this.youhuiIndex].couponId)
				if (this.order[this.youhuiIndex].couponId) {
					uni.showToast({
						title: '重新选择优惠劵，请先清空优惠劵使用',
						icon: 'none'
					})
					return
				}
				for (var i = 0; i < this.order.length; i++) {
					if (this.order[i].couponId == e.id) {
						uni.showToast({
							title: '优惠劵已选择，不能重复使用',
							icon: 'none'
						})
						return
					}
				}
				if (Number(price) >= Number(e.minMoney)) {
					// let totalprice = parseFloat(this.order.money * this.number).toFixed(0)
					this.totalMoney = parseFloat(price - e.money).toFixed(2)
				} else {
					uni.showToast({
						title: '使用优惠劵，下单金额必须大于' + e.minMoney + '元',
						icon: 'none'
					})
					return
				}
				// console.log( e.couponId)
				// return
				this.couponId = e.id
				this.order[this.youhuiIndex].couponMoney = e.money
				this.order[this.youhuiIndex].couponId = e.id
				this.order[this.youhuiIndex].couponName = e.money
				console.log(this.order[this.youhuiIndex].couponId)
				// this.youhuiMoney = e.money
				this.openWay = 3
				// this.couponId = e.id
				// this.couponName = e.money
				this.showYouhuijuan = false

			},
			getMyList() {
				let data = {
					// page: this.page,
					// limit: this.limit,
					status: 0
				}
				this.$Request.getT('/app/couponUser/getMyCouponList', data).then(res => {
					if (res.code == 0) {
						// let data = res.data.records
						// for (var i = 0; i < data.length; i++) {
						// 	data[i].expirationTime = data[i].expirationTime.substring(0, 10)
						// }
						if (this.page == 1) {
							this.youhuiList = res.data.records
						} else {
							this.youhuiList = [...this.youhuiList, ...res.data.records]
						}
					}
				});
			},
			getWaterlist() {
				let data = {
					page: this.page,
					limit: this.limit,
					relationId: this.id
				}
				this.$Request.get("/app/ticketsUserRole/getMyTicketsList", data).then(res => {
					uni.hideLoading();
					if (res.code == 0) {
						// item.stock-item.num
						if (res.data.records.length != 0) {
							let number = res.data.records
							this.water = Number(number[0].stock) - Number(number[0].num)
							this.water = Number(number[0].stock) - Number(number[0].num)
							this.roleId = number[0].roleId
							if (this.page == 1) {
								this.waterList = res.data.records
							} else {
								this.waterList = [...this.waterList, ...res.data.records]
							}
						}
					} else {
						console.log(res.msg)
					}
					uni.stopPullDownRefresh();
				})
			},
			//支付选择
			selectWay: function(item) {
				console.log(item.id)
				this.openWay = item.id;
			},
			openTiem(index) {
				this.timeIndex = index
				this.show = true
			},
			getUserInfo() {
				this.$Request.get("/app/user/selectUserById").then(res => {
					if (res.code == 0) {
						this.bucket = res.data.bucket ? res.data.bucket : '0'
					}
				});
			},
			Orderpay() {
				if (!this.detailaddress) {
					uni.showToast({
						title: '请选择服务地址',
						icon: 'none',
						duration: 1000
					})
					return
				}
				// if (!this.laundryId) {
				// 	uni.showToast({
				// 		title: '请选择附近站点',
				// 		icon: 'none',
				// 		duration: 1000
				// 	})
				// 	return
				// }
				let flag = true
				this.order.some(val => {
					if (!val.laundryId && val.laundryId == '') {
						uni.showToast({
							title: '当前地址无法下单,请切换地址！',
							icon: 'none',
							duration: 1000
						})
						flag = false
						return true
					}
				})
				let flags = true
				let num = 0
				this.order.map(val => {
					if (val.isNeedBucket == 1) {
						num += Number(val.orderNumber)
					}

					// console.log(val.orderNumber,'lllllll',Number(val.orderNumber) > Number(this.bucket))
					// if ( val.isNeedBucket == 1) {
					// 	console.log(val.orderNumber,'lllllll',Number(val.orderNumber) > Number(this.bucket))

					// 	if (Number(val.orderNumber) > Number(this.bucket)) {
					// 		uni.showToast({
					// 			title: '您当前押桶数量是' +this.bucket  + '，请先去押桶',
					// 			icon: 'none',
					// 			duration: 1000
					// 		})
					// 		flags = false
					// 		return true
					// 	}

					// }
				})
				if (Number(num) > Number(this.bucket)) {
					uni.showModal({
						title: '提示',
						content: '您当前押桶数量是' + this.bucket + '桶，请先去押桶',
						success: function (res) {
							if (res.confirm) {
								// console.log('用户点击确定');
								uni.navigateTo({
									url:'/my/tong/index'
								})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					});
					// uni.showToast({
					// 	title: '您当前押桶数量是' + this.bucket + '，请先去押桶',
					// 	icon: 'none',
					// 	duration: 1000
					// })
					return true
				}
				// num
				// console.log(flag,'11111')
				if (flag) {
					if (flags) {
						this.showpay = true
					}
				}

			},
			// 详情
			getDet() {
				// console.error(this.ordersIds, '---------')
				let data = {
					ordersIds: this.ordersIds.toString()
				}
				this.$Request.get("/app/orders/selectOrderListByOrdersIds", data).then(res => {
					if (res.code == 0) {
						this.order = []
						res.data.forEach(d => {
							d.startTime = '立即配送'
							d.couponId = ''
							d.couponName = ''
							d.couponMoney = ''
							// d.laundryId = ''
							// d.laundryName = ''
							this.order.push(d);
						});
						this.loading = false;
					} else {
						uni.showToast({
							title: res.msg,
							duration: 1000,
							icon: 'none'
						});
					}
				});
			},
			statusChange(e) {

				this.startTime = e.year + '-' + e.month + '-' + e.day + ' ' + e.hour + ':' + e.minute
				this.order[this.timeIndex].startTime = this.startTime
			},
			// 根据地址id查询地址
			getAddressList(addressId) {
				if (addressId) {
					this.$Request.getT('/app/address/selectAddressByAddressId?addressId=' + this.addressId).then(res => {
						console.log(res)
						if (res.code == 0) {
							this.name = res.data.name;
							this.mobile = res.data.phone;
							// this.cityaddress = res.data.province + res.data.city + res.data.district;
							this.detailaddress = res.data.detailsAddress;
							this.isDefault = res.data.isDefault;
							this.userId = res.data.userId;
							this.latitude = res.data.latitude;
							this.longitude = res.data.longitude;
							this.province = res.data.province
							this.city = res.data.city
							this.district = res.data.district
							let id = ''
							for (var i = 0; i < this.orderList.length; i++) {
								id = this.orderList[i].orderTakingId
								this.getzhandian(id);
							}
						}
						// uni.hideLoading();
					});
				} else {
					
					uni.navigateTo({
						url: '/my/address/address?id=' + 1
					})
				}
			},
			// 获取默认地址
			addressMy() {
				this.$Request.getT('/app/address/selectAddressById').then(res => {
					console.log(res)
					if (res.code == 0) {
						this.name = res.data.name;
						this.mobile = res.data.phone;
						// this.cityaddress = res.data.province + res.data.city + res.data.district;
						this.detailaddress = res.data.detailsAddress;
						this.isDefault = res.data.isDefault;
						this.userId = res.data.userId;
						this.latitude = res.data.latitude;
						this.longitude = res.data.longitude;
						this.province = res.data.province
						this.city = res.data.city
						this.district = res.data.district
						this.addressId = res.data.addressId
						let id = ''
						for (var i = 0; i < this.orderList.length; i++) {
							id = this.orderList[i].orderTakingId
							this.getzhandian(id);
						}

					}
				});
			},
			pay() {
				let that = this
				that.formData = []
				that.order.forEach(val => {
					var arr = {}
					arr.ordersId = val.ordersId
					arr.startTime = val.startTime
					arr.province = that.province
					arr.city = that.city
					arr.district = that.district
					arr.detailsAddress = that.detailaddress
					arr.name = that.name
					arr.phone = that.mobile
					arr.latitude = that.latitude
					arr.longitude = that.longitude
					// roleId
					arr.roleId = that.roleId
					arr.couponId = val.couponId
					arr.laundryId = that.laundryId
					that.formData.push(arr)
				})

				let orders = that.formData
				that.showpay = false;
				if (that.openWay == 2) { //微信
					uni.showModal({
						title: '付款提示',
						content: '确认支付' + that.totalMoney + '元吗?',
						complete(re) {
							if (re.confirm) {
								// #ifdef MP-WEIXIN
								that.$Request.postJson("/app/wxPay/wxPayShoppingOrders/3", orders).then(
									red => {
										if (red.code == 0) {
											uni.requestPayment({
												provider: 'wxpay',
												timeStamp: red.data.timestamp,
												nonceStr: red.data.noncestr,
												package: red.data.package,
												signType: red.data.signType,
												paySign: red.data.sign,
												success: function(redd) {
													uni.hideLoading();
													uni.showToast({
														title: '支付成功'
													})
													setTimeout(function() {
														uni.redirectTo({
															url: '/my/order/index?index=0'
														})
													}, 1000)
													uni.removeStorageSync(
														'carlist')
												},
												fail: function(err) {
													uni.hideLoading();
													that.$queue.showToast(
														'支付失败');
												}
											});
										} else {
											uni.showToast({
												title: red.msg,
												icon: 'none'
											})
										}
									});
								// #endif

								// #ifdef H5
								that.$Request.postJson("/app/wxPay/wxPayShoppingOrders/2", orders).then(
									red => {
										if (red.code == 0) {
											that.callPay(red);
										} else {
											uni.showToast({
												title: red.msg,
												icon: 'none'
											})
										}
									});
								// #endif

								// #ifdef APP-PLUS
								that.$Request.postJson("/app/wxPay/wxPayShoppingOrders/1", orders).then(
									red => {
										if (red.code == 0) {
											console.log(JSON.stringify(red))
											that.setPayment('wxpay', JSON.stringify(red
												.data));
										} else {
											uni.showToast({
												title: red.msg,
												icon: 'none'
											})
										}
									});
								// #endif
							}
						}
					})
				} else if (that.openWay == 1) { //支付宝
					uni.showModal({
						title: '付款提示',
						content: '确认支付' + that.totalMoney + '元吗?',
						complete(re) {
							if (re.confirm) {
								// #ifdef H5
								that.$Request.postJson("/app/aliPay/payShoppingOrders/2", orders).then(red => {
									if (red.code == 0) {
										const div = document.createElement('div')
										div.innerHTML = red.data //此处form就是后台返回接收到的数据
										document.body.appendChild(div)
										document.forms[0].submit()
									} else {
										uni.showToast({
											title: red.msg,
											icon: 'none'
										})
									}
								});
								// #endif
								// #ifdef APP-PLUS
								that.$Request.postJson("/app/aliPay/payShoppingOrders/1", orders).then(red => {
									if (red.code == 0) {
										that.setPayment('alipay', red.data);
									} else {
										uni.showToast({
											title: red.msg,
											icon: 'none'
										})
									}
								});
								// #endif
							}
						}
					})

				} else if (that.openWay == 3) { //零钱
					uni.showModal({
						title: '付款提示',
						content: '确认支付' + that.totalMoney + '水贝吗?',
						complete(re) {
							if (re.confirm) {
								that.$Request.postJson("/app/orders/payShopping", orders).then(res => {
									if (res.code == 0) {
										uni.showToast({
											title: '支付成功'
										})
										setTimeout(function() {
											uni.redirectTo({
												url: '/my/order/index?index=0'
											})

										}, 1000)
										uni.removeStorageSync('EditAddress')
									} else {
										uni.showToast({
											title: res.msg,
											icon: 'none'
										})
									}
								});
							}
						}
					})

				} else if (that.openWay == 4) { //水票支付
					this.openWay = 1;
					// if (this.water >= this.number) {

					uni.showModal({
						title: '付款提示',
						content: '确认使用水票支付？',
						complete(re) {
							if (re.confirm) {
								// console.log(orders,'---------')
								// return
								that.$Request.postJson("/app/orders/ticketPayMoney", orders).then(res => {
									if (res.code == 0) {
										uni.showToast({
											title: '支付成功'
										})
										setTimeout(function() {
											uni.redirectTo({
												url: '/my/order/index?index=0'
											})
										}, 1000)
										uni.removeStorageSync('EditAddress')
									} else {
										uni.showToast({
											title: res.msg,
											icon: 'none'
										})
									}
								});
							}
						}
					})
					// } else {
					// 	uni.showToast({
					// 		title: '水票不足，请先去购买水票',
					// 		icon: 'none'
					// 	})
					// 	return
					// }
				}
			},
			callPay: function(response) {
				if (typeof WeixinJSBridge === "undefined") {
					if (document.addEventListener) {
						document.addEventListener('WeixinJSBridgeReady', this.onBridgeReady(response), false);
					} else if (document.attachEvent) {
						document.attachEvent('WeixinJSBridgeReady', this.onBridgeReady(response));
						document.attachEvent('onWeixinJSBridgeReady', this.onBridgeReady(response));
					}
				} else {
					this.onBridgeReady(response);
				}
			},
			onBridgeReady: function(response) {
				let that = this;
				if (!response.package) {
					return;
				}
				WeixinJSBridge.invoke(
					'getBrandWCPayRequest', {
						"appId": response.appid, //公众号名称，由商户传入
						"timeStamp": response.timestamp, //时间戳，自1970年以来的秒数
						"nonceStr": response.noncestr, //随机串
						"package": response.package,
						"signType": response.signType, //微信签名方式：
						"paySign": response.sign //微信签名
					},
					function(res) {
						if (res.err_msg === "get_brand_wcpay_request:ok") {
							// 使用以上方式判断前端返回,微信团队郑重提示：
							//res.err_msg将在用户支付成功后返回ok，但并不保证它绝对可靠。
							uni.hideLoading();
							uni.showToast({
								title: '支付成功'
							})
							setTimeout(function() {
								uni.redirectTo({
									url: '/my/order/index?index=0'
								})

							}, 1000)
							uni.removeStorageSync('carlist')
						} else {
							uni.hideLoading();
						}
						WeixinJSBridge.log(response.err_msg);
					}
				);
			},
			setPayment(name, order) {
				let that = this;
				uni.requestPayment({
					provider: name,
					orderInfo: order, //微信、支付宝订单数据
					success: function(res) {
						uni.hideLoading();
						uni.showToast({
							title: '支付成功'
						})
						setTimeout(function() {
							uni.redirectTo({
								url: '/my/order/index?index=0'
							})

						}, 1000)
						uni.removeStorageSync('carlist')
					},
					fail: function(err) {
						uni.hideLoading();
						console.log(12)
					}
				});
			}
		},
	}
</script>

<style>
	page {
		background: #f5f5f5;
	}

	.bg {
		background: #ffffff;
	}

	.foot {
		position: fixed;
		bottom: 0upx;
		left: 0upx;
		right: 0upx;
		z-index: 99;
	}

	.shopcart {
		color: #ffffff;
		background: #025EFD;
		padding: 35upx 60upx;
		text-align: center;
	}

	.pay_btn {
		width: 90%;
		margin: 0 auto;
		text-align: center;
		background: #025EFD;
		height: 80rpx;
		border-radius: 16rpx;
		color: #ffffff;
		line-height: 80rpx;
		margin-bottom: 50upx;
	}

	.listbox {
		/* background: #FFFFFF; */
		background: #F5F5F5;
		margin: 30rpx 30rpx;
		border-radius: 24rpx;
		padding: 30rpx 0;
	}

	.btn {
		color: #FD3C44;
		border: 1rpx solid #FD3C44;
		border-radius: 55upx;
		padding: 8upx 25upx;
	}

	.noyouhui {
		color: #FD3C44;
		border: 1rpx solid #FD3C44;
		border-radius: 15upx;
		display: inline-block;
		padding: 5rpx 20rpx;
	}
</style>