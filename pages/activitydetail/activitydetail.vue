<template>
	<view style="padding: 20rpx;margin-top: 180rpx;">
		<!-- 顶部自定义导航 -->
		<tn-nav-bar fixed alpha customBack>
			<view slot="back" class='tn-custom-nav-bar__back' @click="goBack()">
				<text class='icon tn-icon-left'></text>
				<text class='icon tn-icon-home-capsule-fill'></text>
			</view>
		</tn-nav-bar>
		<view class="box" v-if="activity.id">
			<view class="title_img">
					<image :src="activity.cover" mode="widthFix" style="width: 550rpx;"></image>
			</view>
			<view class="line" style="margin-top: 40rpx;">
				<view class="line-title">活动标题：</view>
				<view class="line-content">{{ activity.name }}</view>
			</view>

			<view class="line">
				<view class="line-title">活动简介：</view>
				<view class="line-content">{{ activity.descr }}</view>
			</view>
			<view class="line">
				<view class="line-title">活动日期：</view>
				<view class="line-content">{{ activity.start }} ~ {{ activity.end }}</view>
			</view>
			<view class="line" style="margin-bottom: 40rpx;">
				<view class="line-title">活动地址：</view>
				<view class="line-content">{{ activity.address }}</view>
			</view>

			<view style="margin-top: 20rpx;">
				<button @click="handleSign()" type="primary" class="my-button-primary"
					v-if="activity.status === '未报名'">开始报名</button>
				<button type="primary" class="my-button-primary" v-if="activity.status === '已报名'" disabled
					key="sign">已报名</button>
				<button type="primary" class="my-button-primary" v-if="activity.status === '未开始'" disabled
					key="notstart">活动未开始</button>
				<button type="primary" class="my-button-primary" v-if="activity.status === '已结束'" disabled
					key="notend">活动已结束</button>
				<!-- <button type="primary" class="my-button-primary" @click="back">确认</button> -->
			</view>

			<view style="margin: 50upx 0;">
				<view style="margin-bottom: 30upx; font-size: 30upx; font-weight: bold; text-align: center;">
					-- 活动详情 --
				</view>
				<view class="w-e-text" style="">
					<rich-text :nodes="activity.content"></rich-text>
				</view>
			</view>

			<!-- <view class="line" style="width: 600px;">
				<view style="width: 100%;overflow: hidden;" v-html="activity.content"></view>

			</view> -->

			<view>
				<uni-popup ref="inputDialog" type="dialog">
					<uni-popup-dialog ref="inputClose" :value="form.phone" @confirm="handleConfirm">
						<view style="font-size: 30rpx;">请确认是否报名</view>

					</uni-popup-dialog>
				</uni-popup>

			</view>
			<!-- <view>
				
				<uni-popup ref="inputDialog" type="dialog">
					<uni-popup-dialog ref="inputClose" mode="input" title="输入内容" value="对话框预置提示内容!" placeholder="请输入内容"
						@confirm="dialogInputConfirm"></uni-popup-dialog>
				</uni-popup>
			</view> -->

		</view>
	</view>
</template>

<script>
	import Comment from "@/libs/components/Comment.vue";
	export default {
		components: {
			Comment
		},
		data() {
			return {
				id: '',
				activity: {
					content: "<p>活动详情内容</p>"
				},
				activityId: '',
				fromVisible: false,
				form: {

				},
				user: uni.getStorageSync('xm-user'),
				rules: {
					phone: [{
						required: true,
						message: '手机号必填',
						trigger: 'blur'
					}]
				}
			};
		},

		onLoad(option) {
			const activityId = option.activityId
			this.load(activityId)
		},
		methods: {
			back() {
				uni.navigateBack()
			},
			load(activityId) {
				this.$request.get('/activity/selectById/' + activityId).then(res => {
					this.activity = res.data || {}
				})
			},
			goBack() {
				setTimeout(() => {
					uni.navigateTo({
						url: '/pages/activity/activity'
					})
				}, 500)
			},
			handleConfirm() {
				this.sign(); // 调用 sign() 方法
			},
			sign() {
				this.$request.post('/activitySign/add', this.form).then(res => {
					if (res.code === '200') {
						uni.showToast({
							title: '报名成功',
							icon: 'success'
						});
						this.fromVisible = false;
						this.load(this.form.activityId);
					} else {
						uni.showToast({
							title: res.msg || '报名失败',
							icon: 'none'
						});
					}
				}).catch(error => {
					console.error(error);
					uni.showToast({
						title: '报名失败',
						icon: 'none'
					});
				});
			},
			handleSign() {
				this.$refs.inputDialog.open();
				this.form = {};
				this.form.userId = this.user.id;
				this.form.phone = this.user.phone;
				this.form.activityId = this.activity.id;
				this.fromVisible = true;
			},

			getUserInfo() {
				// 获取用户信息
				const userInfo = uni.getStorageSync('xm-user') || '{}';
				this.user = JSON.parse(userInfo);
			}
		}

	}
</script>

<style lang="scss" scoped>
	
	.title_img {
		// background-color: red;
		text-align: center;
	}

	
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

	/* 胶囊*/
	.tn-custom-nav-bar__back {
		width: 100%;
		height: 100%;
		position: relative;
		display: flex;
		justify-content: space-evenly;
		align-items: center;
		box-sizing: border-box;
		background-color: rgba(0, 0, 0, 0.15);
		border-radius: 1000rpx;
		border: 1rpx solid rgba(255, 255, 255, 0.5);
		color: #FFFFFF;
		font-size: 18px;

		.icon {
			display: block;
			flex: 1;
			margin: auto;
			text-align: center;
		}

		&:before {
			content: " ";
			width: 1rpx;
			height: 110%;
			position: absolute;
			top: 22.5%;
			left: 0;
			right: 0;
			margin: auto;
			transform: scale(0.5);
			transform-origin: 0 0;
			pointer-events: none;
			box-sizing: border-box;
			opacity: 0.7;
			background-color: #FFFFFF;
		}
	}


	/* 底部悬浮按钮 start*/
	.tn-tabbar-height {
		min-height: 100rpx;
		height: calc(120rpx + env(safe-area-inset-bottom) / 2);
	}

	.tn-footerfixed {
		position: fixed;
		width: 100%;
		bottom: calc(60rpx + env(safe-area-inset-bottom));
		z-index: 1024;
		box-shadow: 0 1rpx 6rpx rgba(0, 0, 0, 0);

	}

	/* 底部悬浮按钮 end*/

	/* 卡 */
	.button-vip {
		width: 100%;
		height: 300rpx;
		border-radius: 15rpx;
		position: relative;
		z-index: 1;

		&::after {
			content: " ";
			position: absolute;
			z-index: -1;
			width: 100%;
			height: 100%;
			left: 0;
			bottom: 0;
			border-radius: inherit;
			opacity: 1;
			transform: scale(1, 1);
			background-size: 100% 100%;
			background-image: url(https://resource.tuniaokj.com/images/cool_bg_image/icon_bg4.png);
		}
	}

	.w-e-text {
		line-height: 40upx;
		text-indent: 2em;
	}
	
	.w-e-text img{
		text-indent: 0em;
		width: 500upx;
	}

	/* 间隔线 start*/
	.tn-strip-bottom-min {
		width: 100%;
		border-bottom: 1rpx solid #F8F9FB;
	}

	.tn-strip-bottom {
		width: 100%;
		border-bottom: 20rpx solid rgba(241, 241, 241, 0.8);
	}

	/* 间隔线 end*/
</style>