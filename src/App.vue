<template>
    <div id="app" class="app-main-contain">
        <div class="center-contain" style="width: 100%; height: calc(100% - 25px)">
            <SplitPane @resize="leftResize" :default-percent="leftPercent" split="vertical">
                <template slot="paneL">
                    <div class="title-contain" v-show="leftPercent > 0">
                        <p>资源导航</p>
                        <i class="iconfont icon-zuidahua" v-show="leftPercent > 0 && leftPercent < 100" @click="leftMaxFunc()"></i>
                        <i class="iconfont icon-zuixiaohua" v-show="leftPercent >= 100" @click="leftMinFunc()"></i>
                    </div>
                    1111111
                </template>
                <template slot="paneR">
                    <SplitPane split="vertical" @resize="rightResize" :default-percent="rightPercent">
                        <template slot="paneL">
                            <SplitPane split="horizontal" @resize="topBottomResize" :default-percent="topPercent">
                                <template slot="paneL">
                                    <div class="title-contain" v-show="topPercent > 0 && rightPercent > 0">
                                        <p>查询编辑器</p>
                                        <i class="iconfont icon-zuidahua" v-show="leftPercent !== 100 && rightPercent !== 100" @click="topMaxFunc()"></i>
                                        <i class="iconfont icon-zuixiaohua" v-show="leftPercent == 0 && topPercent == 100 && rightPercent == 100" @click="topMinFunc()"></i>
                                    </div>
                                    <div style="height: calc(100% - 25px)">2222222</div>
                                </template>
                                <template slot="paneR">
                                    <div class="title-contain" v-show="topPercent < 100 && rightPercent > 0">
                                        <p>结果区</p>
                                        <i class="iconfont icon-zuidahua" v-show="leftPercent !== 100 && topPercent !== 0 && rightPercent !== 0" @click="bottomMaxFunc()"></i>
                                        <i class="iconfont icon-zuixiaohua" v-show="leftPercent == 0 && topPercent == 0 && rightPercent >= 100" @click="bottomMinFunc()"></i>
                                    </div>
                                    <div v-show="topPercent < 100 && rightPercent > 0" style="height: calc(100% - 25px)">3333333</div>
                                </template>
                            </SplitPane>
                        </template>
                        <template slot="paneR">
                            <div class="title-contain" v-show="rightPercent < 100">
                                <p>辅助区</p>
                                <i class="iconfont icon-zuidahua" v-show="leftPercent !== 100 && rightPercent !== 0" @click="rightMaxFunc()"></i>
                                <i class="iconfont icon-zuixiaohua" v-show="leftPercent == 0 && rightPercent == 0" @click="rightMinFunc()"></i>
                            </div>
                            <div v-show="rightPercent < 100" style="height: calc(100% - 25px)">444444</div>
                        </template>
                    </SplitPane>
                </template>
            </SplitPane>
        </div>
        <div class="buttom-contain">
            <BottomTaskBar :rightPercent="rightPercent" :leftPercent="leftPercent" :topPercent="topPercent" @leftClose="leftClose" @rightClose="rightClose" @bottomClose="bottomClose" @openBottom="openBottom" @openLeft="openLeft" @openRight="openRight"></BottomTaskBar>
        </div>
    </div>
</template>

<script>
    import SplitPane from './components/SplitPane.vue'

    export default {
        name: 'index',
        components: {
            SplitPane,
            BottomTaskBar: () => import('./components/BottomTaskBar.vue'),
        },
        created() {
            this.leftPercent = parseInt(localStorage.getItem('leftPercent') || 20)
            this.rightPercent = parseInt(localStorage.getItem('rightPercent') || 70)
            this.topPercent = parseInt(localStorage.getItem('topPercent') || 70)
        },
        data() {
            return {
                GLayoutRoot: null,
                leftPercent: 20,
                rightPercent: 70,
                topPercent: 70,
                oldLeftPercent: 20,
                oldRightPercent: 70,
                oldTopPercent: 70,
            }
        },
        watch: {
            leftPercent: {
                handler(percent) {
                    console.log('watchleftPercent', percent)
                    localStorage.setItem('leftPercent', percent)
                },
                deep: true,
            },
            rightPercent: {
                handler(percent) {
                    console.log('watchrightPercent', percent)
                    localStorage.setItem('rightPercent', percent)
                },
                deep: true,
            },
            topPercent: {
                handler(percent) {
                    console.log('watchtopPercent', percent)
                    localStorage.setItem('topPercent', percent)
                },
                deep: true,
            },
        },
        methods: {
            leftResize(percent) {
                this.oldLeftPercent = percent
                this.leftPercent = percent
            },
            rightResize(percent) {
                this.oldRightPercent = percent
                this.rightPercent = percent
            },
            topBottomResize(percent) {
                this.oldTopPercent = percent
                this.topPercent = percent
            },
            leftClose() {
                this.leftPercent = 0
            },
            rightClose() {
                this.rightPercent = 100
            },
            bottomClose() {
                this.topPercent = 100
            },
            openBottom() {
                this.topPercent = this.oldTopPercent
            },
            openLeft() {
                this.leftPercent = this.oldLeftPercent
            },
            openRight() {
                this.rightPercent = this.oldRightPercent
            },
            leftMaxFunc() {
                this.leftPercent = 100
            },
            leftMinFunc() {
                this.leftPercent = this.oldLeftPercent
            },
            topMaxFunc() {
                this.topPercent = 100
                this.leftPercent = 0
                this.rightPercent = 100
            },
            topMinFunc() {
                this.topPercent = this.oldTopPercent
                this.leftPercent = this.oldLeftPercent
                this.rightPercent = this.oldRightPercent
            },
            bottomMaxFunc() {
                this.topPercent = 0
                this.leftPercent = 0
                this.rightPercent = 100
            },
            bottomMinFunc() {
                this.topPercent = this.oldTopPercent
                this.leftPercent = this.oldLeftPercent
                this.rightPercent = this.oldRightPercent
            },
            rightMaxFunc() {
                this.leftPercent = 0
                this.rightPercent = 0
            },
            rightMinFunc() {
                this.topPercent = this.oldTopPercent
                this.leftPercent = this.oldLeftPercent
                this.rightPercent = this.oldRightPercent
            },
        },
    }
</script>

<style scoped>
    .app-main-contain {
        height: 100%;
        display: flex;
        flex-direction: column;
    }

    .center-contain .title-contain {
        display: flex;
        align-items: center;
        padding: 0 10px 0 10px;
        justify-content: space-between;
        background-color: #eee;
    }

    .center-contain .title-contain p {
        margin: 0;
        height: 25px;
        line-height: 25px;
        font-size: 12px;
    }

    .center-contain .title-contain i {
        font-size: 14px;
        cursor: pointer;
    }

    .app-main-contain .buttom-contain {
        position: fixed;
        bottom: 0;
        height: 25px;
        width: 100%;
        background-color: #0074c8;
        z-index: 999999;
    }
</style>