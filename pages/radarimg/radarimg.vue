<template>
    <view class="wrap" @tap="pageTap">
        <view style="height: 40upx;"></view>
        <view class="choose_source">
            <picker @change="source_change" :value="cityIndex" :range="sourceArray">
                <text class="choose-source-header text">数据源:&nbsp;&nbsp;</text>
                <text class="text text-blue">{{sourceArray[cityIndex]}}&nbsp;&nbsp;</text>
                <text class="text text-blue fa fa-angle-down"></text>
            </picker>
        </view>
        <view class="separator"></view>
        <picSwiperAlt :imgArray="imageArray" :startIndex="startIndex" :autoStart="autostart" interval="3000" ref="picswiper"></picSwiperAlt>
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
                cityIndex: 0,
                data_source: '青岛',
                sourceArray: ['青岛', '烟台', '潍坊', '威海', '滨州'],
                imageArray: [],
                dateArray: [],
                startIndex: 0,
                autostart: false
            }
        },
        computed: {
            // 当前城市名称
            cityName: {
                get() { return this.$store.state.Datas.cityname }
            }
        },
        methods: {
            //切换数据源
            source_change(e) {
                this.cityIndex = e.detail.value
                this.data_source = this.translateCities(this.sourceArray[e.detail.value])
                this.requestImage(this.data_source)
            },
            // 向服务器请求图片
            requestImage(source) {
                let that = this
                uni.request({
                    url: appsettings.hosturl + 'GetRadarImages_0905',
                    data: { name: 'admin', areaflg: '山东', cityname: source },
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
            },
            // 将城市名转换为接口有效参数
            translateCities (cityName) {
                switch (cityName) {
                    case '青岛':
                        return '青岛'
                    case '烟台':
                        return '烟台'
                    case '潍坊':
                        return '潍坊'
                    case '威海':
                        return '荣成'
                    case '滨州':
                        return '滨州'
                    default:
                        return '青岛'
                }
            }
        },
        onLoad() {
            let that = this
            switch (this.cityName) {
                case '青岛':
                    that.cityIndex = 0
                    break
                case '烟台':
                    that.cityIndex = 1
                    break
                case '潍坊':
                    that.cityIndex = 2
                    break
                case '威海':
                    that.cityIndex = 3
                    break
                case '滨州':
                    that.cityIndex = 4
                    break
                default:
                    that.cityIndex = 0
                    break
            }
            this.data_source = this.translateCities(this.cityName)
            this.requestImage(this.data_source)
        },
        onPullDownRefresh() {
			console.log('[界面]: 雷达图 下拉刷新')
            this.requestImage(this.data_source)
            uni.stopPullDownRefresh()
		},
    }
</script>

<style scoped>
    @import "../../common/FontAwesome.css";
    @import "../../common/generic.css";

    Page {
        width: 100%;
        height: 100%;
    }

    .wrap {
        background: #f0eff5;
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