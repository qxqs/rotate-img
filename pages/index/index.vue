<template>
	<view class="content">
		<view class="image-block">
			<view class="image-item">
				<image class="img" :src="item.url" v-for="(item,index) in imgList" :key="index" mode="heightFix">
				</image>
			</view>
		</view>
		<view class="bg-img" v-for="(item,index) in bgList" :key="index"
			v-show="index>=activeIndex&&index<activeIndex+2">
			<image class="img" :src="item.url" mode="heightFix"></image>
		</view>

		<image src="../../static/yy.png" mode="widthFix" class="yinyue-icon" :class="{active:isPlay}" @click="onClick">
		</image>
		<image src="../../static/fx.png" mode="widthFix" class="yinyue-icon fx" @click="onShare">
		</image>

	</view>
</template>

<script>
	const db = uniCloud.database()
	const usersDB = db.collection('image_list')
	export default {
		data() {
			return {
				activeIndex: -1,
				isPlay: false,
				imgList: [],
				bgList: [],
				innerAudioContext: null,
				value: ''
			}
		},
		async onLoad(data) {
			if (data && data.value) {
				this.value = data.value
				let res = await usersDB.where({
					value: data.value
				}).get()
				if (res.result && res.result.data[0]) {
					this.imgList = res.result.data[0].headerList;
					this.bgList = res.result.data[0].bgList;
					this.innerAudioContext = uni.createInnerAudioContext();
					this.innerAudioContext.autoplay = true;
					this.isPlay = true
					this.innerAudioContext.loop = true;
					this.innerAudioContext.src = res.result.data[0].audio.length > 0 ? res.result.data[0].audio[0]
						.url :
						'https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-hello-uniapp/2cc220e0-c27a-11ea-9dfb-6da8e309e0d8.mp3';
				}
			}
			setInterval(() => {
				if (this.activeIndex == this.bgList.length - 2) {
					this.activeIndex = -1
				}
				this.activeIndex++
			}, 4000)

		},
		methods: {
			onClick() {
				this.isPlay = !this.isPlay
				if (this.isPlay) {
					this.innerAudioContext.play(() => {
						console.log('开始播放');
					});

				} else {
					this.innerAudioContext.pause(() => {
						console.log('暂停');
					});

				}
			},
			onShare() {
				uni.setClipboardData({
					data: 'https://static-2459ca4e-abb2-4a68-93e9-860286e4d57b.bspapp.com/#/pages/index/index?value=' +
						this.value,
					success: function() {
						console.log('success');
					}
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.content {
		position: relative;
		height: 100%;
		padding-top: 200rpx;
		overflow: hidden;
	}

	.yinyue-icon {
		position: absolute;
		top: 50rpx;
		right: 30rpx;
		width: 30px;
		cursor: pointer;
	}

	.fx {
		top: 150rpx;
	}

	.active {
		animation: image-block 3s infinite linear;
	}

	@keyframes rotate {
		0% {
			transform: rotateY(0);
		}

		100% {
			transform: rotateY(360deg);
		}
	}

	@keyframes image-block {
		from {
			transform: rotateZ(0);
		}

		to {
			transform: rotateZ(-360deg);
		}
	}

	.image-block {
		width: 100%;
		height: 150px;
		transform-style: preserve-3d;
		transform-origin: center center;
		animation: image-block 18s infinite linear;
		overflow: hidden;
	}

	.image-item {
		position: relative;
		width: 250rpx;
		height: 100%;
		margin: 0 auto;
		transform-style: preserve-3d;
		transform-origin: center center;
		animation: rotate 8s linear infinite;

		/* 图片路径 */
		.img {
			width: 100%;
			height: 100%;
			position: absolute;
			top: 0;
			left: 0;
			opacity: 0.7;
		}
	}


	.image-item .img:nth-child(1) {
		/* 在z轴上移动200px */
		transform: rotateY(0deg) translateZ(250rpx);
	}

	.image-item .img:nth-child(2) {
		/* 在y轴上旋转60度，然后在z轴上移动300像素，下面类似 */
		transform: rotateY(90deg) translateZ(250rpx);

	}

	.image-item .img:nth-child(3) {
		transform: rotateY(180deg) translateZ(250rpx);

	}

	.image-item .img:nth-child(4) {
		transform: rotateY(270deg) translateZ(250rpx);
	}

	.image-item .img:nth-child(5) {
		transform: rotateY(0deg);
	}

	.image-item .img:nth-child(6) {
		transform: rotateY(360deg);

	}

	@keyframes bg-img1 {
		0% {
			opacity: 0;
		}


		50% {
			opacity: 1;
		}

		100% {
			opacity: 0;
		}
	}

	@keyframes bg-img2 {
		0% {
			opacity: 0;
		}


		50% {
			opacity: 1;
		}

		100% {
			opacity: 0;
		}
	}

	.bg-img {
		height: 100%;
		position: absolute;
		top: 0;
		left: 0;
		z-index: -1;

		.img {
			height: 100%;

			&:nth-child(2n) {

				animation: bg-img2 8s infinite linear;
			}

			&:nth-child(2n-1) {
				// opacity: 0.2;
				animation: bg-img1 8s infinite linear;
			}
		}
	}

	// .bg-img-active {
	// 	position: absolute;
	// 	animation: bg-img1 8s infinite linear;
	// }

	@media (min-width: 1000px) {

		.image-item {
			width: 400rpx;
		}

		.image-item .img:nth-child(1) {
			/* 在z轴上移动200px */
			transform: translateZ(400rpx);
		}

		.image-item .img:nth-child(2) {
			/* 在y轴上旋转60度，然后在z轴上移动300像素，下面类似 */
			transform: rotateY(90deg) translateZ(400rpx);

		}

		.image-item .img:nth-child(3) {
			transform: rotateY(180deg) translateZ(400rpx);

		}

		.image-item .img:nth-child(4) {
			transform: rotateY(270deg) translateZ(400rpx);
		}
	}
</style>
