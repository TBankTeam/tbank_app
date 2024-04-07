<template>
	<view style="padding: 20rpx;margin-top: 180rpx;">
		<!-- 顶部自定义导航 -->
		<tn-nav-bar fixed alpha customBack>
			<view slot="back" class='tn-custom-nav-bar__back' @click="goBack()">
				<text class='icon tn-icon-left'></text>
				<text class='icon tn-icon-home-capsule-fill'></text>
			</view>
		</tn-nav-bar>
		<view class="box" v-if="news.id">
			<view style="margin: 50upx 0;">
				<view style="margin-bottom: 20upx; font-size: 40upx; font-weight: bold; text-align: center;">
					{{ news.title }}
				</view>
				<view style="margin-bottom: 10upx; font-size: 22upx; font-weight: bold; text-align: center;">
					发布时间：{{ news.time }}
				</view>
				<view style="margin-bottom: 25upx; font-size: 22upx; font-weight: bold; text-align: center;">
					浏览量：{{ news.count }}
				</view>
				<view class="w-e-text" style="">
					<rich-text :nodes="news.content"></rich-text>
				</view>
			</view>
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
				news: {
					content: "<p>活动详情内容</p>"
				},
				newsId: '',
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
			const newsId = option.newsId
			this.load(newsId)
		},
		methods: {
			back() {
				uni.navigateBack()
			},
			load(newsId) {
				this.$request.get('/news/selectById/' + newsId).then(res => {
					this.news = res.data || {}
				})
			},
			goBack() {
				setTimeout(() => {
					uni.navigateTo({
						url: '/pages/news/news'
					})
				}, 500)
			},
			handleConfirm() {
				this.sign(); // 调用 sign() 方法
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
	
	.w-e-text {
		line-height: 45upx;
		text-indent: 2em;
	}
	
	.w-e-text img{
		text-indent: 0em;
		width: 500upx;
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