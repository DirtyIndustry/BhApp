<template>
    <view class="page-body menu-page" catchtouchmove>
        <image src="../../static/Images/back_images.png" mode="aspectFill" style="width: 100%; height: 100%; position: fixed; top: 0; left: 0; z-index: -1;" />
        <view class="menu-table">
            <!-- 第一行 -->
            <view class="menu-row">
                <!-- 第一行左 -->
                <view class="menu-cell menu-column">
                    <view class="menu-button" hover-class="hover" @tap="oceanforecastTap">
                        <image class="menu-button-icon" mode="aspectFit" src="../../static/Images/area_img.png" />
                        <view class="text-large text-green">海区预报</view>
                    </view>
                </view>
                <!-- 第一行右 -->
                <view class="menu-cell menu-column">
                    <view class="menu-button" hover-class="hover" @tap="warninglistTap">
                        <image class="menu-button-icon" mode="aspectFit" src="../../static/Images/Warning_img.png" />
                        <view class="text-large text-green">预警报</view>
                    </view>
                </view>
            </view>
            <!-- 第二行 -->
            <view class="menu-row">
                <!-- 第二行左 -->
                <view class="menu-cell menu-column">
                    <view class="menu-button" hover-class="hover" @tap="cityforeTap">
                        <image class="menu-button-icon" mode="aspectFit" src="../../static/Images/city_img.png" />
                        <view class="text-large text-green">城市预报</view>
                    </view>
                </view>
                <!-- 第二行右 -->
                <!-- <view class="menu-cell menu-column">
                    <view class="menu-button" hover-class="hover" @tap="trendTap">
                        <image class="menu-button-icon" mode="aspectFit" src="../../static/Images/trendPrediction_img.png" />
                        <view class="text-large text-green">趋势预测</view>
                    </view>
                </view> -->
                <view class="menu-cell menu-column">
                    <view class="menu-button" hover-class="hover" @tap="newscenterTap">
                        <image class="menu-button-icon" mode="aspectFit" src="../../static/Images/news_img.png" />
                        <view class="text-large text-green">新闻中心</view>
                    </view>
                </view>
            </view>
            <!-- 第三行 -->
            <view class="menu-row">
                <!-- 第三行左 -->
                <view class="menu-cell menu-column">
                    <view class="menu-button" hover-class="hover" @tap="weatherTap">
                        <image class="menu-button-icon" mode="aspectFit" src="../../static/Images/WeatherForecast_img.png" />
                        <view class="text-large text-green">天气预报</view>
                    </view>
                </view>
                <!-- 第三行右 -->
                <view class="menu-cell menu-column">
                    <view class="menu-button" hover-class="hover" @tap="weatherimgTap">
                        <image class="menu-button-icon" mode="aspectFit" src="../../static/Images/WeatherMap_img.png" />
                        <view class="text-large text-green">天气图</view>
                    </view>
                </view>
            </view>
            <!-- 第四行 -->
            <view class="menu-row">
                <!-- 第四行左 -->
                <view class="menu-cell menu-column">
                    <view class="menu-button" hover-class="hover" @tap="satelliteimgTap">
                        <image class="menu-button-icon" mode="aspectFit" src="../../static/Images/SatelliteCloudMap_img.png" />
                        <view class="text-large text-green">卫星云图</view>
                    </view>
                </view>
                <!-- 第四行右 -->
                <view class="menu-cell menu-column">
                    <view class="menu-button" hover-class="hover" @tap="radarimgTap">
                        <image class="menu-button-icon" mode="aspectFit" src="../../static/Images/RadarMap_img.png" />
                        <view class="text-large text-green">雷达图</view>
                    </view>
                </view>
            </view>
            <!-- 第五行 -->
            <view class="menu-row">
                <!-- 第五行左 -->
                <view class="menu-cell menu-column">
                    <view class="menu-button" hover-class="hover" @tap="typhoonpathTap">
                        <image class="menu-button-icon" mode="aspectFit" src="../../static/Images/Typhoon_img.png" />
                        <view class="text-large text-green">台风路径</view>
                    </view>
                </view>
                <!-- 第五行右 -->
                <view class="menu-cell menu-column">
                    <view class="menu-button" hover-class="hover" @tap="fisheryTap">
                        <image class="menu-button-icon" mode="aspectFit" src="../../static/Images/fishery_img.png" />
                        <view class="text-large text-green">渔场预报</view>
                    </view>
                </view>
            </view>
        </view>
    </view>
</template>

<script>
export default {
    methods: {
        // 城市预报
        cityforeTap (e) {
            console.log('城市预报点击')
            uni.navigateTo({
                // url: '../cityforecast/cityforecast'
                url: '../cityforecastAlt/cityforecastAlt'
            })
        },
        // 预警报
        warninglistTap (e) {
            console.log('预警报点击')
            uni.navigateTo({
                url: '../warninglist/warninglist'
            })
        },
        // 渔场预报
        fisheryTap (e) {
            console.log('渔场预报点击')
            uni.navigateTo({
                url: '../fisheryforecast/fisheryforecast'
            })
        },
        // 趋势预测
        trendTap (e) {
            console.log('趋势预测点击')
            uni.navigateTo({
                url: '../trendprediction/trendprediction'
            })
        },
        // 天气预报
        weatherTap (e) {
            console.log('天气预报点击')
            uni.navigateTo({
                url: '../weatherforecast/weatherforecast'
            })
        },
        // 天气图
        weatherimgTap (e) {
            console.log('天气图点击')
            uni.navigateTo({
                url: '../weatherimg/weatherimg'
            })
        },
        // 卫星云图
        satelliteimgTap (e) {
            console.log('卫星云图点击')
            uni.navigateTo({
                url: '../satelliteimg/satelliteimg'
            })
        },
        // 雷达图
        radarimgTap (e) {
            console.log('雷达图点击')
            uni.navigateTo({
                url: '../radarimg/radarimg'
            })
        },
        // 台风路径
        typhoonpathTap (e) {
            console.log('台风路径点击')
            uni.navigateTo({
                url: '../typhoonmap/typhoonmap'
            })
        },
        // 新闻中心
        newscenterTap (e) {
            console.log('新闻中心点击')
            uni.navigateTo({
                url: '../newscenter/newscenter'
            })
        },
        // 海区预报
        oceanforecastTap (e) {
            console.log('海区预报点击')
            uni.navigateTo({
                url: '../oceanforecast/oceanforecast'
            })
        }
    }
}
</script>

<style scoped>
    @import "../../common/generic.css";

    .menu-page {
        height: 100%;
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .menu-table {
        height: 85%;
        width: 90%;
        display: flex;
        flex-direction: column;
    }

    .menu-row {
        flex: 1;
        display: flex;
        flex-direction: row;
    }

    .menu-column {
        flex: 1;
    }

    .menu-cell {
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .menu-button {
        width: 300upx;
        height: 120upx;
        display: flex;
        align-items: center;
        justify-content: center;
        background-color: rgba(255, 255, 255, 0.8);
        border-radius: 15upx;
        box-shadow: 0 8upx 0 rgba(6, 135, 132, 0.4);
    }

    .hover {
        background-color: rgba(221, 221, 221, 0.8);
        box-shadow: 0 6upx 0 rgba(6, 135, 132, 0.4);
    }

    .menu-button-icon {
        width: 50upx;
        height: 50upx;
    }

</style>
