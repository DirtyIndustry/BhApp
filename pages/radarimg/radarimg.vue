<template>
    <view class="wrap" @tap="pageTap">
        <view style="height: 40upx;"></view>
        <view class="choose_source">
                <text class="choose-source-header text">数据源:&nbsp;&nbsp;</text>
                <text class="text text-green">中国气象网&nbsp;&nbsp;</text>
        </view>
        <view class="separator"></view>
        <picSwiperAlt :imgArray="imageArray" :startIndex="startIndex" :autoStart="autostart" interval="1500" ref="picswiper"></picSwiperAlt>
    </view>
</template>

<script>
    import appsettings from '../../utils/appsettings.js'
    import picSwiperAlt from '../../components/picSwiperAlt.vue'

    export default {
        components: {
            picSwiperAlt
        },
        data() {
            return {
                imageArray: [],
                dateArray: [],
                startIndex: 0,
                autostart: false
            }
        },
        methods: {
            // 向服务器请求图片
            requestImage() {
                let that = this
                uni.request({
                    url: appsettings.hosturl + 'GetRadarImages_0905',
                    data: { name: 'admin', areaflg: '北海', cityname: '' },
                    method: 'POST',
                    success: function (res) {
                        console.log('[服务器]: 返回 雷达图')
                        if (!res.data.d | res.data.d === '当前没数据') {
                            console.log('[服务器]: 返回 雷达图 返回值为空')
                            return false
                        }
                        let dataarr = JSON.parse(res.data.d)
                        // 将返回数据填入数组
                        let imgarr = []
                        let datearr = []
                        for (let i = 0; i < dataarr.length; i++) {
                            imgarr.push(dataarr[i].url)
                            datearr.push(dataarr[i].title)
                        }
                        that.startIndex = dataarr.length - 1
                        that.imageArray = imgarr
                        that.dateArray = datearr
                    }
                })
            }, // end-requestImage()
            // 页面点击
            pageTap() {
                this.$refs.picswiper.timerStart()
            }
        },
        onLoad() {
            this.requestImage()
        },
        onPullDownRefresh() {
			console.log('[界面]: 雷达图 下拉刷新')
            this.requestImage()
            uni.stopPullDownRefresh()
		},
    }
</script>

<style scoped>
    @import "../../common/generic.css";

    Page {
        width: 100%;
        height: 100%;
    }

    .wrap {
        background: #f7f7f7;
        width: 100%;
        height: 100%;
    }

    .choose_source {
        width: 100%;
        height: 80upx;
        background-color: #fff;
        line-height: 80upx;
    }

    .choose-source-header {
        margin-left: 100upx;
    }
    
</style>