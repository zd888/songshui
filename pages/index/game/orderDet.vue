<template>
	<view style="padding-bottom: 80px;">
		<view class=" u-skeleton-fillet" style="position: relative;">
			<view class="bg u-flex" @click="getAddressList()" style="padding: 25rpx 25rpx 20rpx 25rpx; ">
				<view class="u-flex-1 text-white margin-left-xs">
					<view class="padding-tb" v-if="detailaddress ==''">
						<view class="text-lg text-bold margin-bottom-xs">服务地址</view>
						<view style="color: #999999;">请选择服务地址</view>
					</view>
					<view v-else>
						<view class="u-font-16  text-bold">{{province}} {{city}} {{district}}{{detailaddress}}</view>
						<view class="u-font-14 margin-top-xs u-tips-color flex justify-between">
							<view style="color: #333333;">{{name}}<text class="margin-left-sm">{{mobile}}</text>
							</view>
						</view>
					</view>
				</view>
				<u-icon name="arrow-right" color="#999999"></u-icon>
			</view>
			<image src="../../../static/images/index/line.png" style="width: 100%;height: 6upx;position: absolute;">
			</image>
		</view>
		<view class="padding-tb">
			<view class=" bg  u-skeleton-fillet  " style="padding: 25rpx;">
				<view class="flex" style="position: relative;">
					<image :src="order.homepageImg?order.homepageImg:'../../../static/logo.png'"
						style="width: 160upx;height: 160upx;" mode="aspectFill"></image>
					<view class="u-flex-1 text-white margin-left-xs">
						<view class="  u-line-1" style="width: 480rpx;">{{order.serviceName}}</view>
						<view class="flex align-center margin-tb-sm text-sm" style="color: #999999;">
							<view>{{attrName}} {{skuMessage}}</view>
							<view class="margin-left-xs">数量 x{{number}}</view>
						</view>
						<view style="color: #FF2D01;font-size: 32upx;font-weight: bold;">
							<text class="text-sm">￥</text>{{price}}
						</view>
					</view>

				</view>
			</view>
			<view class="padding bg margin-top  u-skeleton-fillet">
				<view class="flex align-center justify-between margin-bottom">
					<view class="text-30" style="color: #1E1F31;width: 240upx;">数量</view>
					<view class="flex justify-between ">
						<view class="flex align-center ">
							<view class="jia" @click="down(number)">-</view>
							<view class="content">{{number}}</view>
							<view class="jian" @click="add(number)">+</view>
						</view>
					</view>
				</view>
				<view class="flex align-center justify-between margin-top-xl">
					<view class="text-30" style="color: #1E1F31;width: 240upx;">配送时间</view>
					<view class="flex justify-between " @click="show = true">
						<view>{{startTime}}</view>
					</view>
				</view>
				<view class="flex align-center justify-between margin-top-xl">
					<view class="text-30" style="color: #1E1F31;width: 240upx;">附近站点</view>
					<view class="flex justify-between">
						<view>{{laundryName?laundryName:'请选择附近站点'}}</view>
					</view>
				</view>
				<!-- 	<view class="flex align-center justify-between margin-top" v-if="waterList.length!=0">
					<view class="text-30" style="color: #1E1F31;width: 240upx;">水票</view>
					<view class="flex align-center" @click="showShuipiao = true">
						<view>{{watertitle?watertitle:'立即使用'}}</view>
						<image src="../../../static/images/my/right.png"
							style="width: 12rpx;height: 24rpx;margin-left: 10rpx;"></image>
					</view>
				</view> -->

				<view class="flex align-center justify-between margin-top-xl padding-bottom"
					v-if="youhuiList.length!=0">
					<view class="text-30" style="color: #1E1F31;width: 240upx;">优惠劵</view>
					<view class="flex align-center" @click="showYouhuijuan = true">
						<view>{{-couponName?-couponName:'立即使用'}}</view>
						<image src="../../../static/images/my/right.png"
							style="width: 12rpx;height: 24rpx;margin-left: 10rpx;"></image>
					</view>
				</view>
			</view>


		</view>
		<view class="text-white flex justify-between cu-bar foot bg " style="color: #1A1A1A;">
			<view class="flex align-center padding-lr">
				<view>合计：</view>
				<view style="color: #FD3C44;">
					￥<text style="font-size: 48upx;font-weight: bold;">{{totalprce}}</text>
				</view>
			</view>
			<view class="buttn cu-bar" @click="orderpay()">确认下单</view>
		</view>

		<u-picker v-model="show" mode="time" :params="params" @confirm="statusChange"></u-picker>

		<u-popup v-model="showPay" mode="bottom" border-radius="14" :closeable="true">
			<view
				style="width: 100%;text-align: center;font-size:38rpx;font-weight: bold;margin-top:15rpx;margin-bottom: 80rpx;">
				选择支付方式
			</view>
			<view style="display: flex;height: 100upx;align-items: center;padding: 20upx 30rpx;"
				v-for="(item,index) in openLists" :key='index'>
				<view style="display: flex;width:80%;align-items: center;">
					<image :src="item.image" style="width: 55upx;height: 55upx;border-radius: 50upx;"></image>
					<view style="font-size: 30upx;margin-left: 20upx;width: 70%;">{{item.text}}
					</view>
				</view>
				<view style="width: 20%;display: flex;justify-content: flex-end;">
					<radio-group name="openWay" style="margin-left: 20upx;" @tap='selectWay(item.id)'>
						<label class="tui-radio">
							<radio color="#1789FD" :checked="openWay === item.id ? true : false" />
						</label>
					</radio-group>
				</view>
			</view>

			<view style="display: flex;height: 100upx;align-items: center;padding: 20upx 30rpx;" v-if="isShuibei!='否'"
				v-for="(item,index) in openListl" :key='index'>
				<view style="display: flex;width:80%;align-items: center;">
					<image :src="item.image" style="width: 55upx;height: 55upx;border-radius: 50upx;"></image>
					<view style="font-size: 30upx;margin-left: 20upx;width: 70%;">{{item.text}}
					</view>
				</view>
				<view style="width: 20%;display: flex;justify-content: flex-end;">
					<radio-group name="openWay" style="margin-left: 20upx;" @tap='selectWay(item.id)'>
						<label class="tui-radio">
							<radio color="#1789FD" :checked="openWay === item.id ? true : false" />
						</label>
					</radio-group>
				</view>
			</view>
			<view style="display: flex;height: 100upx;align-items: center;padding: 20upx 30rpx;"
				v-if="waterList.length!=0&&!couponId" v-for="(item,index) in openList" :key='index'>
				<view style="display: flex;width:80%;align-items: center;">
					<image src="../../../static/images/shuipiao.png"
						style="width: 55upx;height: 55upx;border-radius: 50upx;"></image>
					<view style="font-size: 30upx;margin-left: 20upx;width: 70%;">{{item.name}}
						<text style="font-size: 26rpx;margin-left:15rpx;">剩余{{water}}张</text>
					</view>
				</view>
				<view style="width: 20%;display: flex;justify-content: flex-end;">
					<radio-group name="openWay" style="margin-left: 20upx;" @tap='selectWay(item.id)'>
						<label class="tui-radio">
							<radio color="#1789FD" :checked="openWay === item.id ? true : false" />
						</label>
					</radio-group>
				</view>
			</view>
			<view
				style="width: 690rpx;height: 80rpx;background:#1789FD;color:#FFFFFF;text-align: center;line-height: 80rpx;border-radius: 50rpx;margin: 30rpx;"
				@click="pay()">确认支付</view>
		</u-popup>

		<!-- <u-popup v-model="showShuipiao" mode="bottom" border-radius="14" :closeable="true">
			<view style="background: #F5F5F5;height: 100%;padding-bottom: 30rpx;">
				<view
					style="width: 100%;text-align: center;font-size:38rpx;font-weight: bold;padding-top:15rpx;padding-bottom:40rpx;">
					选择水票
				</view>
				<view class="waterbox" v-for="(item,index) in waterList" :key='index'>
					<view class="padding-lr padding-bottom text-lg text-bold" style="color: #000000;">{{item.title}}
					</view>
					<view class="padding-top" style="border-top: 1rpx dashed #E5E5E5;">
						<view class="flex align-center justify-between padding-lr ">
							<view class="text-26" style="color: #999999;">剩余{{item.stock-item.num}}张</view>
							<view class="btn" @click="waterpay(item.roleId,item.title)">立即使用</view>
						</view>
					</view>
				</view>
			</view>

		</u-popup> -->

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
		<!-- <u-skeleton :loading="loading" :animation="true" elColor='#FFFFFF' bgColor='#FFFFFF'></u-skeleton> -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				loading: false, // 是否显示骨架屏组件

				id: '',
				order: {},
				isVip: false,


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

				longitude: '',
				latitude: '',
				startHour: '',
				showPay: false,
				openWay: 3,
				openLists: [],
				openList: [{
					id: 4,
					name: '水票'
				}],
				openListl: [{
					image: '../../../static/images/lingqian.png',
					text: '水贝',
					id: 3
				}],
				detailaddress: '',
				name: '',
				mobile: '',
				province: '',
				city: '',
				district: '',

				check: false,
				number: '',
				skuMessage: '',
				skuId: '',
				attrName: '',
				number: '',
				price: 0,
				totalprce: 0,
				skuId: '',
				isNumber: false,

				// showShuipiao: false,
				water: '',
				waterList: [],
				roleId: '',
				// watertitle: '',
				laundryName: '',
				laundryId: '',
				showYouhuijuan: false,
				youhuiList: [],
				couponId: '',
				couponName: '',
				youhuiMoney: 0,
				page: 1,
				limit: 10,
				isShuibei: '否',
				bucket: 0
			}
		},
		onLoad(option) {
			// console.log(option)
			this.id = option.id
			this.attrName = option.attrName
			this.price = option.price
			this.number = option.number
			this.skuMessage = option.skuMessage
			this.totalprce = parseFloat(this.price * this.number)
			this.skuId = option.skuId
			this.isVip = uni.getStorageSync('isVIP')

			this.getDet()
			this.getWaterlist()
			this.getMyList();
			// #ifdef APP-PLUS
			this.openLists = [{
				image: '../../../static/images/zhifubao.png',
				text: '支付宝',
				id: 1
			}, {
				image: '../../../static/images/icon_weixin.png',
				text: '微信',
				id: 2
			}];
			this.openWay = 1;
			// #endif

			// #ifdef MP-WEIXIN
			this.openLists = [{
				image: '../../../static/images/icon_weixin.png',
				text: '微信',
				id: 2
			}];
			this.openWay = 2;
			// #endif

			// #ifdef H5
			this.openLists = [{
				image: '../../../static/images/zhifubao.png',
				text: '支付宝',
				id: 1
			}];
			this.openWay = 1;
			// #endif
		},
		destryed() {
			// uni.removeStorageSync('carlist')
		},
		onHide() {
			// uni.removeStorageSync('carlist')
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
			// this.carlist = uni.getStorageSync('carlist')
			// this.carNo = this.carlist.carNo
			// this.carName = this.carlist.carName
			// this.carPhone = this.carlist.carPhone
		},
		methods: {
			getUserInfo() {
				this.$Request.get("/app/user/selectUserById").then(res => {
					if (res.code == 0) {
						this.bucket = res.data.bucket ? res.data.bucket : '0'
					}
				});
			},
			getzhandian(longitude, latitude) {
				this.$Request.get("/app/laundry/selectLaundryByDistance", {
					latitude: this.latitude,
					longitude: this.longitude,
					goodsId: this.id
				}).then(res => {
					if (res.status == 0) {
						// this.laundry = res.data
						if (res.data) {
							if (res.data.distance <= res.data.max_scope) {
								this.laundryName = res.data.laundry_name;
								this.laundryId = res.data.laundry_id;
							} else {
								this.laundryName = '';
								this.laundryId = '';
								this.$queue.showToast('当前站点无法下单,请切换地址！');
								// this.$queue.showToast('当前附近' + res.data.max_scope + '米内没有师傅可以服务！');
							}
						} else {
							this.$queue.showToast('当前站点无法下单,请切换地址！');
							// this.$queue.showToast('当前附近没有师傅可以服务！');
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
			qingkong() {
				this.couponId = ''
				this.couponName = ''
				this.totalprce = parseFloat(this.order.money * this.number).toFixed(2)
				this.showYouhuijuan = false
			},
			youhuiPay(e) {
				let price = this.totalprce
				// console.log(price, price >= e.minMoney)
				if (Number(price) >= Number(e.minMoney)) {

					let totalprice = parseFloat(this.order.money * this.number).toFixed(2)
					// console.log(totalprice, e.money, totalprice - e.money)
					let price = parseFloat(totalprice - e.money).toFixed(2)
					// console.log(totalprice, e.money, totalprice - e.money, price < 0)
					if (price < 0) {
						this.totalprce = 0.01
					} else {
						this.totalprce = price
					}

				} else {
					uni.showToast({
						title: '使用优惠劵，下单金额必须大于' + e.minMoney + '元',
						icon: 'none'
					})
					return
				}

				this.youhuiMoney = e.money
				this.openWay = 3
				this.couponId = e.id
				this.couponName = e.money
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

			down(index) {
				if (this.number == 1) {
					uni.showToast({
						title: '下单数量不能低于1个',
						icon: 'none'
					})
					return
				} else {
					this.number--
					if (this.couponId) {
						// console.log(this.couponId,'---------------',this.youhuiMoney)
						let money = parseFloat(this.order.money * this.number).toFixed(2)
						this.totalprce = parseFloat(money - this.youhuiMoney).toFixed(2)
					} else {
						this.totalprce = parseFloat(this.order.money * this.number).toFixed(2)
					}

				}
			},
			add(num) {
				this.number = Number(num) + 1
				if (this.couponId) {
					let money = parseFloat(this.order.money * this.number).toFixed(2)
					// console.log(money,'+++++++++++',this.youhuiMoney)
					this.totalprce = parseFloat(Number(money) - Number(this.youhuiMoney)).toFixed(2)
				} else {
					this.totalprce = parseFloat(this.order.money * this.number).toFixed(2)
				}
				// this.totalprce = parseFloat(this.order.money * this.number).toFixed(0)
				// this.number++
			},
			checkboxGroupChange(e) {
				// console.log(e, '--------');
			},
			// 选中某个复选框时，由checkbox时触发
			checkboxChange(e) {
				console.log(e);
			},
			// 根据地址id查询地址
			getAddressList(addressId) {
				if (addressId) {
					this.$Request.getT('/app/address/selectAddressByAddressId?addressId=' + this.addressId).then(res => {
						// console.log(res)
						if (res.code == 0) {
							this.name = res.data.name;
							this.mobile = res.data.phone;
							this.cityaddress = res.data.province + res.data.city + res.data.district;
							this.detailaddress = res.data.detailsAddress;
							this.isDefault = res.data.isDefault;
							this.userId = res.data.userId;
							this.latitude = res.data.latitude;
							this.longitude = res.data.longitude;
							this.province = res.data.province
							this.city = res.data.city
							this.district = res.data.district

							this.getzhandian(this.longitude, this.latitude);
						}
						// uni.hideLoading();
					});
				} else {
					uni.navigateTo({
						url: '../../../my/address/address?id=' + 1
					})
				}
			},
			// 获取默认地址
			addressMy() {
				this.$Request.getT('/app/address/selectAddressById').then(res => {
					// console.log(res)
					if (res.code == 0) {
						this.name = res.data.name;
						this.mobile = res.data.phone;
						this.cityaddress = res.data.province + res.data.city + res.data.district;
						this.detailaddress = res.data.detailsAddress;
						this.isDefault = res.data.isDefault;
						this.userId = res.data.userId;
						this.latitude = res.data.latitude;
						this.longitude = res.data.longitude;
						this.province = res.data.province
						this.city = res.data.city
						this.district = res.data.district
						this.addressId = res.data.addressId
						this.getzhandian(this.longitude, this.latitude);
						// this.$queue.setData('EditAddress', res.data.addressId);
					}
				});
			},
			selectWay: function(id) {
				this.openWay = id;
			},
			statusChange(e) {
				// const date = new Date();
				// let hour = date.getHours() < 10 ? "0" + date.getHours() : date.getHours();
				// let year = date.getFullYear();
				// let month = date.getMonth() + 1 < 10 ? "0" + (date.getMonth() + 1) : date.getMonth() + 1;
				// let day = date.getDate() < 10 ? "0" + date.getDate() : date.getDate();
				// this.startHour = hour
				// console.log(e, '0000000', year,year>= e.year )
				// if (this.startHour >= e.hour) {
				// 	uni.showToast({
				// 		title: '洗车时间必须大于当前时间一小时以上',
				// 		icon: "none"
				// 	})
				// 	return
				// }
				this.startTime = e.year + '-' + e.month + '-' + e.day + ' ' + e.hour + ':' + e.minute
			},

			// 详情
			getDet() {
				this.$Request.get("/app/orderTaking/queryTakingDetails", {
					id: this.id
				}).then(res => {
					if (res.code == 0) {
						this.order = res.data

						if (res.data.goodsAttrList.length != 0) {
							this.isNumber = true
						} else {
							this.isNumber = false
						}
						if (this.order.gameName) {
							this.order.gameName = res.data.gameName.split(',')
						}
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
			orderpay() {

				if (!this.detailaddress) {
					uni.showToast({
						title: '请选择服务地址',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (!this.laundryId) {
					uni.showToast({
						title: '当前地址无法下单,请切换地址！',
						icon: 'none',
						duration: 1000
					})
					return
				}

				if (this.order.isNeedBucket == 1) {
					// console.log(Number(this.number) , Number(this.bucket),'Number(this.number) >= Number(this.bucket)',Number(this.number) >= Number(this.bucket))
					if (Number(this.number) > Number(this.bucket)) {
						uni.showModal({
							title: '提示',
							content: '您当前押桶数量是' + this.bucket + '桶，请先去押桶',
							success: function(res) {
								if (res.confirm) {
									// console.log('用户点击确定');
									uni.navigateTo({
										url: '/my/tong/index'
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
						return
					}
					this.showPay = true;
				} else {
					this.showPay = true;
				}

			},
			pay() {
				uni.showLoading({
					title: '支付中',
				})
				let that = this
				that.$Request.get("/app/orders/generateOrder", {
					id: that.order.id,
					type: 1,
					startTime: that.startTime,
					orderNumber: that.number,
					skuId: that.skuId,
					detailJson: that.skuMessage,
					province: that.province,
					laundryId: that.laundryId,
					city: that.city,
					district: that.district,
					detailsAddress: that.detailaddress,
					name: that.name,
					phone: that.mobile,
					latitude: that.latitude,
					longitude: that.longitude,
					couponId: that.couponId
				}).then(res => {
					that.showPay = false;
					if (res.code == 0) {
						if (that.openWay == 2) { //微信
							uni.hideLoading();
							uni.showModal({
								title: '付款提示',
								content: '确认支付' + that.totalprce + '元吗?',
								complete(re) {
									if (re.confirm) {
										// #ifdef MP-WEIXIN
										that.$Request.post("/app/wxPay/wxPayOrder", {
											orderId: res.data.ordersId,
											classify: 3
										}).then(red => {
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
										that.$Request.post("/app/wxPay/wxPayOrder", {
											orderId: res.data.ordersId,
											classify: 2
										}).then(red => {
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
										that.$Request.post("/app/wxPay/wxPayOrder", {
											orderId: res.data.ordersId,
											classify: 1
										}).then(red => {
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
							uni.hideLoading();
							uni.showModal({
								title: '付款提示',
								content: '确认支付' + that.totalprce + '元吗?',
								complete(re) {
									if (re.confirm) {
										// #ifdef H5
										that.$Request.post("/app/aliPay/payOrder", {
											orderId: res.data.ordersId,
											classify: 2
										}).then(red => {
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
										that.$Request.post("/app/aliPay/payOrder", {
											orderId: res.data.ordersId,
											classify: 1
										}).then(red => {
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
							uni.hideLoading();
							uni.showModal({
								title: '付款提示',
								content: '确认支付' + that.totalprce + '水贝吗?',
								complete(re) {
									if (re.confirm) {
										that.$Request.post("/app/orders/payMoney", {
											ordersId: res.data.ordersId,
										}).then(res => {
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
							uni.hideLoading();
							if (this.water >= this.number) {
								uni.showModal({
									title: '付款提示',
									content: '确认使用水票支付？',
									complete(re) {
										if (re.confirm) {
											let formData = []
											let orderList = {
												ordersId: res.data.ordersId,
												roleId: that.roleId
											}
											formData.push(orderList)
											let orders = formData
											that.$Request.postJson("/app/orders/ticketPayMoney", orders)
												.then(res => {
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
							} else {
								uni.showToast({
									title: '水票不足，请先去购买水票',
									icon: 'none'
								})
								return
							}


						}
					} else {
						this.$queue.showToast(res.msg);
					}

				});
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
		onReachBottom: function() {
			this.page = this.page + 1;
			// this.getWaterlist()
			this.getMyList()
		},
		onPullDownRefresh: function() {
			this.page = 1;
			// this.getWaterlist()
			this.getMyList()
		},
	}
</script>

<style>
	textarea::-webkit-input-placeholder {
		color: red;
	}

	page {
		background-color: #F5F5F5;
	}

	.bg {
		background-color: #FFFFFF;
	}

	.u-input {
		text-align: right !important;
	}

	.buttn {
		padding: 0upx 40upx;
		background: #0175FE;
		color: #FFFFFF;
	}

	.jia {
		width: 50upx;
		height: 48upx;
		border: 1upx solid #000000;
		text-align: center;
		line-height: 42upx;
	}

	.content {
		width: 55upx;
		height: 48upx;
		/*单侧符合属性设置各边框*/
		border-top: 1upx double #000000;
		border-bottom: 1upx double #000000;
		/* border-left: 10px solid #1a5fad; */
		/* border: 1upx solid #000000; */
		text-align: center;
		line-height: 42upx;
	}

	.jian {
		width: 50upx;
		height: 48upx;
		border: 1upx solid #000000;
		text-align: center;
		line-height: 42upx;
	}

	.waterbox {
		background: #FFFFFF;
		margin: 0rpx 30rpx;
		border-radius: 24rpx;
		padding: 30rpx 0;
	}

	.btn {
		border: 5rpx solid red;
		border-radius: 50rpx;
		color: red;
		padding: 10rpx 30rpx;
		font-size: 28rpx;
		font-weight: bold;
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