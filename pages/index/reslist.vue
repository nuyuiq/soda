<template>
    <view class="content">
        <uni-table stripe>
            <uni-tr>
                <uni-th>资源</uni-th>
                <uni-th>说明</uni-th>
            </uni-tr>
            <!-- 表格数据行 -->
            <uni-tr v-for="(item, index) in reslist" :key="index">
                <uni-td>{{ item.res }}</uni-td>
                <uni-td>{{ item.desc }}</uni-td>
            </uni-tr>
        </uni-table>        
    </view>
</template>

<script>
    import uniTable from '@/components/uni-table/uni-table.vue'
    export default {
        data() {
            const list = [{
                    res: '图标',
                    desc: '部分在 Pixabay 上收集'
                },{
                    res: 'iconfont',
                    desc: 'colorUI 中的 icon: cuIcon'
                }]
            return {
                reslist: list
            }
        },
        components: {
            uniTable
        }
    }
</script>

<style>
    .content {
        padding: 10rpx;
    }
</style>