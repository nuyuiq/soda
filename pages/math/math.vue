<template>
    <view v-if="running===0||running===2" class="content">
        <view class="info-txt">
            <text>给大脑除锈</text>
            <text v-for="(txt, idx) in score.txt" :key="idx" class="info-score">{{ txt }}</text>
        </view>
        <button class="info-btn" type="primary" @click="start">{{ running===0? '开始': '重新开始' }}</button>
    </view>
    <view v-else class="content">
        <view class="score">
            <text class="pass-txt">正确：{{ score.pass }}</text>
            <text class="fail-txt">错误：{{ score.fail }}</text>
            <text class="score-txt">得分：{{ score.val }}</text>
            <button type="primary" @click="end">结算</button>
        </view>
        <view class="scene">
            <view class="sym" :class="history.pass? 'his-pass': 'his-fail'">{{ history.txt }}</view>
            <view v-for="(q,idx) in question" :key="q.txt + '.' + idx" class="sym" :class="idx===0?'sym-cur':''">
                {{ q.txt }}
                <text v-if="idx === 0"> = {{ input }} </text>
            </view>
        </view>
        <view class="kbs">
            <text v-for="idx of 11" :key="idx" :class="idx !== 1? 'key-s0': 'key-s1'" @click="clickBtn(idx - 2)">
                {{ idx !== 1? idx - 2: '确定' }}
            </text>
        </view>
    </view>
</template>

<script>
    export default {
        data() {
            const teacher = {
                // 请老师出题
                gen: () => {
                    // 挑选出题老师
                    const total = this.teacher.qs.reduce((p, c) => p + c.wg, 0);
                    let target = Math.random() * total;
                    for (const t of this.teacher.qs) {
                        if (t.wg < target) {
                            target -= t.wg
                        } else {
                            // 请给老师出题
                            return t.gen()
                        }
                    }
                    return {}
                },
                // 题库/老师们
                qs: [
                    {// 加减乘除1位数
                        wg: 3, // 被选中的概率
                        gen: () => {  // 出题
                            let a = this.teacher.genNum(1, 10);
                            let b = this.teacher.genNum(1, 10);
                            let c = this.teacher.genNum(0, 3);
                            if (c === 1) {
                                if (a < b) { let t = a; a = b; b = t; }
                            } else if (c === 3) {
                                if ( a % b !== 0) a = parseInt(a * b);
                            }
                            return {
                                txt: a + [' + ',' - ',' x ',' ÷ '][c] + b,
                                val: c === 3? 2: 1,
                                rs: this.teacher.calc(a, b, c),
                            }
                        }
                    },
                    {// 加减2位数
                        wg: 1,
                        gen: () => {
                            let a = this.teacher.genNum(11, 100);
                            let b = this.teacher.genNum(11, 100);
                            let c = this.teacher.genNum(0, 1);
                            if (c === 1) {
                                if (a < b) { let t = a; a = b; b = t; }
                            }
                            return {
                                txt: a + [' + ',' - '][c] + b,
                                val: 3,
                                rs: this.teacher.calc(a, b, c),
                            }
                        }
                    },
                ],
                // 老师的出题辅助工具
                genNum: (s, e) => { return Math.floor(Math.random() * (e - s + 1)) + s; },
                calc: (a, b, c) => { 
                    if (c === 0) return parseInt(a + b);
                    if (c === 1) return parseInt(a - b);
                    if (c === 2) return parseInt(a * b);
                                 return parseInt(a / b);
                },
            };
            return {
                running: 0,
                score: {
                    txt: [],
                    start: 0,
                    pass: 0,
                    fail: 0,
                    val: 0,
                    rval: 0
                },
                history: {
                    txt: '',
                    pass: true
                },
                question: [],
                teacher: teacher,
                input: ''
            }
        },
        methods: {
            start() {
                this.score.start = new Date().getTime()
                this.score.pass = 0
                this.score.fail = 0
                this.score.val = 0
                this.score.rval = 0
                this.score.txt = []
                this.history.txt = ''
                this.history.pass = true
                this.question = []
                this.updateQs()
                this.running = 1
            },
            end() {
                const time = parseInt((new Date().getTime() - this.score.start) / 1000)
                if (time > 60) {
                    this.score.txt.push('用时 ' + parseInt(time / 60) + ' 分 ' + (time % 60) + ' 秒')
                } else {
                    this.score.txt.push('用时 ' + time  + ' 秒')
                }
                const count = this.score.fail + this.score.pass
                if (count > 0) {
                    this.score.txt.push(parseInt(time / count) + ' 秒/题')
                } else {
                    this.score.txt.push('发呆了 ' + time + ' 秒')
                }
                if (this.score.rval > 0) {
                    this.score.txt.push('得分率 ' + (this.score.val / this.score.rval * 100).toFixed(1) + '%')
                } else {
                    this.score.txt.push('得分率 0%')
                }
                this.score.txt.push(this.score.fail + ' / ' + this.score.pass + ' / ' + this.score.val)
                this.running = 2
            },
            clickBtn(idx) {
                if (idx >= 0) {
                    if (idx !== 0 || this.input.length !== '0') {
                        this.input += idx + ''
                    }
                } else {
                    this.updateQs()
                }
            },
            updateQs() {
                if (this.question.length > 0) {
                    this.updateScore()
                }
                while (this.question.length < 20) {
                    this.question.push(this.teacher.gen())
                }
            },
            updateScore() {
                const val = this.input.length === 0? -1: Number(this.input);
                const q = this.question.shift()
                this.score.rval += q.val
                this.history.txt = q.txt + ' = ' + (val === -1? '?': val)
                if (val === q.rs) {
                    this.score.pass += 1
                    this.score.val += q.val
                    this.history.pass = true
                } else {
                    this.score.fail += 1
                    this.history.pass = false
                    this.history.txt += ' (' + q.rs + ')'
                }
                this.input = ''
            }
        }
    }
</script>

<style>
    .content, .info-txt {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }
    .info-txt {
        position: absolute;
        top: 20%;
        line-height: 2rem;
        font-size: 1.5rem;
        color: dimgray;
    }
    .info-score {
        margin: 10rpx 0rpx;
        font-size: 2rem;
        color: chocolate;
    }
    .info-btn {
        position: absolute;
        top: 60%;
    }
    .score {
        top: 0;
        height: 10%;
        position: absolute;
        display: flex;
        align-items: center;
    }
    .pass-txt {
        color: green;
    }
    .fail-txt {
        color: red;
        margin: 0rem 2rem;
    }
    .score-txt {
        margin-right: 2rem;
    }
    .scene {
        position: absolute;
        bottom: 3%;
        height: 87%;
        width: 90%;
        border-top: 2rpx solid lightgray;
        border-bottom: 2rpx solid lightgray;
        overflow: hidden;
    }
    .sym {
        font-size: 2rem;
        color: lightgray;
        line-height: 2.6rem;
    }
    .sym-cur {
        color: dimgray;
        font-weight: bold;
        line-height: 3rem;
        font-size: 3rem;
    }
    .his-pass {
        color: lightgreen;
    }
    .his-fail {
        color: lightpink;
    }
    .kbs {
        position: absolute;
        bottom: 5%;
        right: 10rpx;
        width: calc(60% - 10rpx);
        display: flex;
        flex-wrap: wrap-reverse;
    }
    .key-s0, .key-s1 {
        margin: 4rpx;
        height: 2.6rem;
        border: 4rpx solid lightskyblue;
        font-size: 2rem;
        font-weight: bold;
        color: dimgray;
        display: flex;
        align-items: center;
        justify-content: center;
        border-radius: 20rpx;
    }
    .key-s0:active, .key-s1:active {
        background-color: lightgray;
    }
    .key-s0 {
        width: calc(100% / 2 - 16rpx);
    }
    .key-s1 {
        width: calc(100% - 16rpx);
        color: blue;
    }
</style>
