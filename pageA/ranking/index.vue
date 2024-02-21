<template>
  <view>
    <view class="u-nav">
      <navbar
        :safeAreaInsetTop="true"
        height="55"
        placeholder
        leftIconColor="#fff"
        autoBack>
        <view class="u-nav-slot" slot="center">
          <u--text text="积分排名" color="#fff"></u--text>
        </view>
      </navbar>
    </view>
    <!-- <mescroll-body ref="mescrollRef" @init="mescrollInit" @down="downCallback" @up="upCallback">
		</mescroll-body> -->

    <view class="list" v-for="(item, index) in lists" :key="index">
      <view class="list-left">
        <view class="pm">{{ index + 1 }}</view>
        <view class="img">
          <image
            src="@/static/assets/me-active.png"
            style="width: 90rpx; height: 90rpx; border-radius: 50%"
            mode="scaleToFill"></image>
        </view>
        <view class="name">{{ item.nickName }}</view>
      </view>
      <view class="list-right">{{ item.points }} 积分</view>
    </view>
  </view>
</template>

<script>
import MescrollMixin from '@/uni_modules/mescroll-uni/components/mescroll-uni/mescroll-mixins.js'
import { ranking } from '@/common/newapi.js'
export default {
  mixins: [MescrollMixin], // 使用mixin
  data() {
    return {
      lists: [],
    }
  },
  onLoad() {
    this.list()
  },
  mounted() {},
  methods: {
    list() {
      uni.request({
        url: 'https://wj.api.cestech.com.cn:48002/mission_b/v1/ranking',
        header: {
          'x-Resource-Token': uni.getStorageSync('xtoken'),
          //'x-Resource-Token':uni.getStorageSync('xtoken'), //自定义请求头信息
        },
        method: 'GET',
        success: res => {
          console.log(res)
          res.data.result.records.forEach(v => {
            console.log(v, 'vvvv')
            if (v.points != 0) {
              this.lists.push(v)
            }
          })
          // this.lists=res.data
        },
      })
    },
  },
}
</script>

<style lang="scss">
page {
  background: #f5f5f5;
}

.list {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  padding: 20rpx 35rpx;
  background: #fff;
  margin-bottom: 15rpx;

  .list-left {
    display: flex;
    align-items: center;

    .pm {
      font-size: 46rpx;
      font-weight: 600;
    }

    .img {
      margin-left: 30rpx;
    }

    .name {
      width: 200rpx;
      overflow: hidden; //超出的文本隐藏
      text-overflow: ellipsis; //溢出用省略号显示
      white-space: nowrap; // 默认不换行；

      font-size: 30rpx;
      font-weight: 500;
      margin-left: 25rpx;
    }
  }

  .list-right {
    color: red;
  }
}
</style>
