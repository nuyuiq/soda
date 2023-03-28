<template>
    <view class="content">
        <image class="avatar" :src="avatar" mode="aspectFit" @click="badge+=1"></image>
        <text class="title">{{ cs }}</text>
        <view class="info">
            <text class="cuIcon cuIcon-mail">{{ email }}</text>
            <text class="cuIcon cuIcon-github">{{ github }}</text>
            <text class="cuIcon cuIcon-home">{{ home }}</text>
        </view>
        <button @click="showResList">资源说明</button>
        <view class="badge" v-if="badge !== 0" @click="badge=0">{{ badge > 99? '99+': badge }}</view>
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
                home: ' nuyuiq.github.io/blog',
                github: ' github.com/nuyuiq',
                badge: 0
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
        margin-top: 160rpx;
        margin-left: auto;
        margin-right: auto;
        margin-bottom: 50rpx;
        border: #cccccc solid 4rpx;
        border-radius: 50%;
    }

    .avatar:active {
        border-width: 6rpx;
        height: 196rpx;
    }
    
    .badge {
        position: absolute;
        top: 160rpx;
        right: calc(100% / 2 - 100rpx);
        border-radius: 50%;
        background-color: red;
        color: #eee;
        width: 1.5rem;
        height: 1.5rem;
        overflow: hidden;
        align-items: center;
        display: flex;
        justify-content: center;
    }

    .title {
        font-size: 2rem;
        color: #8f8f94;
    }
</style>
