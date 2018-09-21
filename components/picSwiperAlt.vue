<template>
    <view class="swiper-body">
        <!-- 图片上方标题 -->
        <view class="pic_title" :class="{pic_title_active: titleArray.length > 0}">{{pictureTitle}}</view>
        <!-- 图片 -->
        <swiper class="imgswiper" :current="imgindex" :autoplay="isPlaying" :interval="interval" duration="0" circular="true" @change="swiperchange"
            :style="{height: viewHeight + 'px'}">
            <swiper-item v-for="(item, index) in imgArray" :key="index">
                <image class="img" :src="item" mode="widthFix" @tap="picTap" @load="imageLoad" />
            </swiper-item>
        </swiper>
        <!-- 图片下方计数器 -->
        <view class="page_number">
            <text>第 {{imgindex + (imgArray.length === 0 ? 0 : 1)}}/{{imgArray.length}} 张</text>
        </view>
        <!-- 播放控制按钮 -->
        <view class="btn_panel" :class="{ btn_panel_hide: isButtonHide }">
            <view class="btn_box">
                <view class="button prev" :class="{btn_click: prevbtnclick}" @tap="prev">
                    <view class="prev-icon fa fa-backward" />
                </view>
                <view class="button play_stop" :class="{btn_click: playbtnclick}" @tap="play_pause">
                    <view class="fa" :class="{'play-icon fa-play': !isPlaying, 'fa-pause': isPlaying}" />
                </view>
                <view class="button next" :class="{btn_click: nextbtnclick}" @tap="next">
                    <view class="next-icon fa fa-forward" />
                </view>
            </view>
        </view>
    </view>
</template>

<script>
    export default {
        name: 'picSwiperAlt',
        props: {
            // 图片列表
            imgArray: {
                type: Array,
                default() {
                    return []
                }
            },
            // 图片标题列表
            titleArray: {
                type: Array,
                default() {
                    return []
                }
            },
            // 起始状态展示第几张图片
            startIndex: {
                type: Number,
                default () {
                    return 0
                }
            },
            // 自动播放
            autoStart: {
                type: Boolean,
                default () {
                    return false
                }
            },
            // 播放间隔
            interval: {
                type: Number,
                default () {
                    return 2000
                }
            }
        },
        data() {
            return {
                pictureTitle: '',
                imgindex: 0,
                isPlaying: false,
                isButtonHide: false,
                viewHeight: '538upx',
                btnTimer: undefined,
                playbtnclick: false,
                nextbtnclick: false,
                prevbtnclick: false,
                timerlist: []
            }
        },
        computed: {
            // 系统信息
            systemInfo: {
                get() { return this.$store.state.Infos.systeminfo }
            },
            // 播放键图标
            playButtonImg: {
                get () {
                    return this.isPlaying ? '../../static/Images/btn_stop.png' : '../../static/Images/btn_play.png'
                }
            }
        },
        watch: {
            imgArray: {
                handler (newVal, oldVal) {
                    if (newVal) {
                        this.imgindex = this.startIndex > newVal.length - 1 ? 0 : this.startIndex
                    }
                }
            },
            titleArray: {
                handler(newVal, oldVal) {
                    if (newVal) {
                        this.initTitle()
                    }
                }
            }
        },
        methods: {
            // 图片预览
            previewImage() {
                uni.previewImage({
                    current: this.imgArray[this.imgindex],
                    urls: this.imgArray
                })
            },
            // 滑动图片
            swiperchange(e) {
                // console.log(e.detail.current)
                this.imgindex = e.detail.current
                this.initTitle()
            },
            // 播放
            play() {
                this.isPlaying = true
            },
            // 停止
            stop() {
                this.isPlaying = false
            },
            // 播放-暂停
            play_pause () {
                if (this.isPlaying === true) {
                    this.isPlaying = false
                } else {
                    this.isPlaying = true
                }
                if (this.playbtnclick !== true) {
                    this.playbtnclick = true
                    let timer = setTimeout(function () {
                        clearTimeout(timer)
                        this.playbtnclick = false
                    }.bind(this), 80)
                    this.timerlist.push(timer)
                }
            },
            // 上一张
            prev() {
                if (this.imgArray.length > 0) {
                    if (this.imgindex === 0) {
                        this.imgindex = this.imgArray.length - 1
                    } else {
                        this.imgindex--
                    }
                    this.initTitle()
                    if (this.prevbtnclick !== true) {
                        this.prevbtnclick = true
                        let timer = setTimeout(function () {
                            clearTimeout(timer)
                            this.prevbtnclick = false
                        }.bind(this), 80)
                        this.timerlist.push(timer)
                    }
                }
            },
            // 下一张
            next() {
                if (this.imgArray.length > 0) {
                    if (this.imgindex < this.imgArray.length - 1) {
                        this.imgindex++
                    } else {
                        this.imgindex = 0
                    }
                    this.initTitle()
                    if (this.nextbtnclick !== true) {
                        this.nextbtnclick = true
                        let timer = setTimeout(function () {
                            clearTimeout(timer)
                            this.nextbtnclick = false
                        }.bind(this), 80)
                        this.timerlist.push(timer)
                    }
                }
            },
            // 初始化图片标题
            initTitle() {
                // 如果标题数组不为空 则显示标题
                if (this.titleArray.length > 0) {
                    if (this.imgindex < this.titleArray.length) {
                        this.pictureTitle = this.titleArray[this.imgindex]
                    } else {
                        this.pictureTitle = ''
                    }
                }
            },
            // 加载图片时设置框体高度
            imageLoad(e) {
                let imgwidth = e.detail.width
                let imgheight = e.detail.height
                let ratio = imgwidth / imgheight
                this.viewHeight = Math.ceil(this.systemInfo.windowWidth / ratio)
                // console.log(this.viewHeight)
            },
            // 启动按钮显隐计时器
            timerStart () {
                let that = this
                this.isButtonHide = false
                if (this.btnTimer !== undefined) {
                    clearTimeout(this.btnTimer)
                    this.btnTimer = undefined
                }
                this.btnTimer = setTimeout(function(){
                    that.isButtonHide = true
                    that.btnTimer = undefined
                }, 6000)
            },
            // 图片点击
            picTap () {
                if (this.isButtonHide === true) {
                    this.timerStart()
                } else {
                    this.previewImage()
                }
                
            }
        }, // end-methods
        onReady() {
            this.imgindex = this.startIndex
            this.isPlaying = this.autoStart
            this.initTitle()
            this.timerStart()
        },
        onUnload () {
            clearTimeout(this.btnTimer)
            this.btnTimer = null
            for (let i = 0; i < this.timerlist.length; i++) {
                clearTimeout(this.timerlist[i])
                this.timerlist[i] = null
            }
        }
    }
</script>

<style scoped>
    @import "../common/FontAwesome.css";

    .swiper-body {
        overflow: hidden;
    }
    
    .pic_title {
        width: 100%;
        height: 80upx;
        font-size: 28upx;
        color: #333;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .pic_title_active {
        background: #fff;
    }

    .page_number {
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 28upx;
        margin: 20upx 0;
    }

    .btn_panel {
        position: absolute;
        bottom: 80upx;
        width: 100%;
        height: 200upx;
        display: flex;
        align-items: center;
        justify-content: center;
        opacity: 100;
    }

    .btn_panel_hide {
        transition: opacity 2s ease, bottom 0s ease 2s, position 0s ease 2s;
        opacity: 0;
        bottom: -200upx;
        position: fixed;
    }

    .btn_box {
        width: 70%;
        left: 15%;
        bottom: 160upx;
        height: 200upx;
        display: flex;
        align-items: center;
        justify-content: space-around;
    }

    .button {
        display: flex;
        align-items: center;
        justify-content: center;
        box-shadow: 0upx 0upx 15upx rgba(0, 0, 0, 0.3);
        color: #666;
        background: -webkit-radial-gradient(#fff, rgb(245, 245, 245), #eee); /* Safari 5.1 - 6.0 */
        background: -o-radial-gradient#fff, (rgb(245, 245, 245), #eee); /* Opera 11.6 - 12.0 */
        background: -moz-radial-gradient(#fff, rgb(245, 245, 245), #eee); /* Firefox 3.6 - 15 */
        background: radial-gradient(#fff, rgb(245, 245, 245), #eee); /* 标准的语法 */
    }
    .btn_click {
        box-shadow: 0upx 0upx 8upx rgba(0, 0, 0, 0.6);
        background: -webkit-radial-gradient(#fff, #ddd, #ccc); /* Safari 5.1 - 6.0 */
        background: -o-radial-gradient#fff, (#ddd, #ccc); /* Opera 11.6 - 12.0 */
        background: -moz-radial-gradient(#fff, #ddd, #ccc); /* Firefox 3.6 - 15 */
        background: radial-gradient(#fff, #ddd, #ccc); /* 标准的语法 */
    }
    .prev,
    .next {
        border-radius: 55upx;
        width: 110upx;
        height: 110upx;
        font-size: 38upx;
    }

    .play_stop {
        border-radius: 70upx;
        width: 140upx;
        height: 140upx;
        font-size: 55upx;
    }

    .prev-icon {
        position: relative;
        right: 5upx;
    }
    .next-icon {
        position: relative;
        left: 5upx;
    }
    .play-icon {
        position: relative;
        left: 5upx;
    }

    .imgswiper {
        width: 100%;
        transition: height .2s ease-in;
    }

    .img {
        width: 100%;
    }
</style>
