<template>
	<view class="pages">
		<view class="header-img">
			<text>请选择尺寸相同的图片，头部旋转</text>
			<text>{{list1.length}}/6</text>
		</view>
		<uni-file-picker :limit="6" v-model="list1" fileMediatype="image" mode="grid" />
		<view class="header-img">
			<text>请选择尺寸相同的图片,用于背景图</text>
		</view>
		<uni-file-picker class="img" v-model="list2" fileMediatype="image" mode="grid" />
		<view class="header-img">
			<text>请选择背景音乐</text>
			<text>{{list1.length}}/1</text>
		</view>
		<uni-file-picker :limit="1" v-model="list3" fileMediatype="all" mode="grid" />

		<button class="btn" @click="onSubmit">确定</button>
	</view>
</template>

<script>
	const db = uniCloud.database()
	const usersDB = db.collection('image_list')
	export default {
		data() {
			return {
				list1: [],
				list2: [],
				list3: [],
				inpValue: ''
			};
		},
		methods: {
			async onSubmit() {

				if (this.list1.length < 4) {
					return uni.showToast({
						title: '用于头部旋转图片不能少于4张',
						icon: 'none'
					});

				}
				if (this.list2.length < 2) {
					return uni.showToast({
						title: '背景图切换图片不能少于2张',
						icon: 'none'
					});

				}
				let value = new Date().getTime()

				let res2 = await usersDB.add({
					headerList: this.list1,
					bgList: this.list2,
					audio: this.list3,
					value: value + ''
				})
				if (res2.result && res2.result.id) {
					uni.navigateTo({
						url: '/pages/index/index?value=' + value
					})
				}
			}
		}
	}
</script>

<style lang="scss">
	.pages {
		padding: 10rpx 20rpx;
	}

	.header-img {
		display: flex;
		justify-content: space-between;
		padding: 10rpx 0;
	}

	.btn {
		margin: 100rpx 0 0;
		background-color: #007aff;
		color: #fff;

	}
</style>
