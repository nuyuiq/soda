<template>
	<view class="content">
        <view class="app" v-for="(app, index) in apps" :key="index" @click="goto(app)">
            <image class="img" :src="app.img" mode="aspectFit"></image>
            <text class="txt">{{ app.name }}</text>
        </view>
    </view>
</template>

<script>
    import applist from '../applist.js'
    export default {
        data() {
            return { apps: applist }
        },
        methods: {
            goto(app) {
                uni.navigateTo({
                    url: app.url
                })
            }
        }
    }
</script>

<style>
    .content {
        display: flex;
        align-items: center;
        flex-wrap: wrap;
        justify-content: center;
        margin-left: 10rpx;
        margin-right: 10rpx;
    }
    .app {
        display: flex;
        align-items: center;
        flex-direction: column;
        width: calc(100vw / 3 - 90rpx);
        height: calc(100vw / 3 - 90rpx + 1rem);
        margin: 30rpx;
        padding: 10rpx;
    }
    .img {
        width: 100%;
        height: calc(100vw / 3 - 90rpx);
        border: 4rpx solid lightgray;
        border-radius: 25%;
    }
    .txt {
        height: 1rem;
        font-size: 1rem;
        color: dimgray;
    }
</style>
