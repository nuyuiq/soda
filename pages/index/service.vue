<template>
	<view class="content">
		<image class="avatar" :src="avatar" mode="aspectFit"></image>
        <text class="title">{{ cs }}</text>
        <view class="info">
            <text class="cuIcon cuIcon-github">{{ github }}</text>
            <text class="cuIcon cuIcon-mail">{{ email }}</text>
            <text class="cuIcon cuIcon-home">{{ home }}</text>
        </view>
        <button @click="showResList">资源说明</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
                // customer service
                cs: '001客服',
                avatar: '/static/img/avatar.png',
                email: ' nuyuiq@163.com',
                home: ' nuyuiq.github.com',
                github: ' github.com/nuyuiq',
			}
		},
		methods: {
            showResList() {
                uni.navigateTo({
                    url: "/pages/index/reslist"
                })
            }
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

    .info {
        display: flex;
        flex-direction: column;
        justify-content: center;
        margin: 2rem 0rem;
        color: #8f8f94;
        font-size: 1rem;
        line-height: 1.5rem;
    }

	.avatar {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
        border: #cccccc solid 4rpx;
        border-radius: 50%;
	}
    
	.title {
		font-size: 2rem;
		color: #8f8f94;
	}
</style>
