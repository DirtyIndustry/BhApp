<template>
    <view class="page-body" catchtouchmove>
        <view class="section-top">
            <view style="height: 100upx;"></view>
            <view class="icon-container">
                <image class="icon-image" src="../../static/Images/icon.png" mode="aspectFit" />
            </view>
            <view class="strip-text text">{{appname}}</view>
            <view class="strip-text text">版本：{{appversion}}</view>
        </view>
        <view class="section-bottom text">国家海洋局北海预报中心&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;版权所有</view>
    </view>
</template>

<script>
import appsettings from '../../utils/appsettings.js'
export default {
    computed: {
        appname () { return appsettings.appname },
        appversion () { return appsettings.appversion }
    }
}
</script>

<style scoped>
.page-body {
    width: 100%;
    height: 100%;
    overflow: hidden;
}

.section-top {
    width: 100%;
    height: 456upx;
    display: flex;
    flex-direction: column;
}

.section-bottom {
    width: 100%;
    height: 200upx;
    position: absolute;
    bottom: 0;
    display: flex;
    align-items: center;
    justify-content: center;
}

.icon-container {
    height: 220upx;
    display: flex;
    align-items: center;
    justify-content: center;
}

.icon-image {
    width: 220upx;
    height: 220upx;
}

.strip-text {
    height: 68upx;
    display: flex;
    align-items: center;
    justify-content: center;
}

.text {
    color: #666;
    font-size: 34upx;
}

</style>
