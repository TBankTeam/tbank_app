<template>
	<view style="padding: 20rpx;margin-top: 180rpx;">
		<view class="box" v-if="orders.id">
			<view class="line" style="margin-bottom: 40rpx;">
				<view class="line-title">订单编号：</view>
				<view class="line-content">{{ orders.orderNo }}</view>
			</view>

			<view v-if="orders.type === '代拿快递'">
				<view class="line">
					<view class="line-title">取货地址：</view>
					<view class="line-content">{{ orders.address.address + orders.address.doorNo }}</view>
				</view>
				<view class="line">
					<view class="line-title">取货联系人：</view>
					<view class="line-content">{{ orders.address.userName }}</view>
				</view>
				<view class="line" style="margin-bottom: 40rpx;">
					<view class="line-title">取货电话：</view>
					<view class="line-content">{{ orders.address.phone }}</view>
				</view>

			</view>

			<view class="line">
				<view class="line-title">服务地址：</view>
				<view class="line-content">{{ orders.targetAddress.address + orders.targetAddress.doorNo }}</view>
			</view>
			<view class="line">
				<view class="line-title">服务联系人：</view>
				<view class="line-content">{{ orders.targetAddress.userName }}</view>
			</view>
			<view class="line" style="margin-bottom: 40rpx;">
				<view class="line-title">服务联系人电话：</view>
				<view class="line-content">{{ orders.targetAddress.phone }}</view>
			</view>

			<view class="line">
				<view class="line-title">服务名称：</view>
				<view class="line-content">{{ orders.name }}</view>
			</view>
			<view class="line">
				<view class="line-title">物品描述：</view>
				<view class="line-content">{{ orders.descr }}</view>
			</view>
			<view class="line">
				<view class="line-title">物品图片：</view>
				<view class="line-content">
					<image :src="orders.img" mode="widthFix" style="width: 180rpx;"></image>
				</view>
			</view>
			<view class="line">
				<view class="line-title">服务类型：</view>
				<view class="line-content">{{ orders.type }}</view>
			</view>
			<!-- <view class="line">
				<view class="line-title">物品重量：</view>
				<view class="line-content">{{ orders.weight }} kg</view>
			</view> -->
			<view class="line">
				​ <view class="line-title">时间币：</view>
				​ <view class="line-content" style="color:red;">{{"￥"+orders.price}}</view>

			</view>
			<view class="line">
				<view class="line-title">服务时间：</view>
				<view class="line-content">{{ orders.datetimesingle }}</view>
			</view>
			<view class="line" style="margin-bottom: 40rpx;">
				<view class="line-title">备注：</view>
				<view class="line-content">{{ orders.comment || '' }}</view>
			</view>

			<view class="line">
				<view class="line-title">志愿者姓名：</view>
				<view class="line-content">{{ orders.certification.name }}</view>
			</view>

			<view class="line">
				<view class="line-title">志愿者电话：</view>
				<view class="line-content">{{ orders.certification.phone }}</view>
			</view>

			<view class="line">
				<view class="line-title">志愿者照片：</view>
				<view class="line-content">
					<view v-if="orders.certification">
						<image :src="orders.certification.avatar" mode="widthFix" style="width: 180rpx;"></image>
					</view>
				</view>
			</view>

			<view style="margin-top: 20rpx;"><button type="primary" class="my-button-primary" @click="back">确认</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				orders: {}
			}
		},
		onLoad(option) {
			const orderId = option.orderId
			this.load(orderId)
		},
		methods: {
			back() {
				uni.navigateBack()
			},
			load(orderId) {
				this.$request.get('/orders/selectById/' + orderId).then(res => {
					this.orders = res.data || {}
				})
			}
		}
	}
</script>

<style>
	.line {
		display: flex;
		margin-bottom: 10rpx;
		grid-gap: 10rpx;
	}

	.line-title {
		width: 200rpx;
		font-weight: bold;
		text-align: right;
	}

	.line-content {
		flex: 1;
	}
</style>