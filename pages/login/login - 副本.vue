<template>
	<view class="">
		<view class="form">

			<view class="form-item">
				<text>用户名：</text>
				<input class="input" id="name" type="text" :value="formData.value">
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				formData: {
					value: '1234'
				}
			};
		}
	}
</script>

<style lang="scss">
	.form {
		padding: 20rpx;
	}

	.form-item {
		display: flex;
		align-items: center;

	}

	.input {
		display: block;
		border: 1px solid #ccc;
		flex: 1;
		height: 60rpx;
		padding: 0 20rpx;
	}
</style>
