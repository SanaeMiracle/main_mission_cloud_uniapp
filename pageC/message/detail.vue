<template>
  <view class="mainBox bg-white">
    <!-- <u-loading-page :loading="pageLoading"></u-loading-page> -->
    <view class="content u-flex-col">
      <view class="title u-flex-1 u-p u-flex-col">
        <view class="title-box">
          <view class="u-m-b">
            <u--text
              align="center"
              type="info"
              size="14"
              :text="detail.createTime">
            </u--text>
          </view>
          <u--text align="center" size="15" :text="detail.content"> </u--text>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
import { messageone } from '@/common/api.js'
import dayjs from 'dayjs'

export default {
  data() {
    return {
      pageLoading: true,
      globalData: getApp().globalData,
      btnLoading: false,
      id: '',
      detail: {
        content: '',
        createTime: '',
      },
    }
  },
  computed: {},
  filters: {},
  onLoad(option) {
    console.log(option)
    this.detail.content = option.content
    this.detail.createTime = option.createTime
    this.id = option.id
    this.reads()
  },
  onReady() {},
  methods: {
    reads() {
      uni.request({
        url: `https://wj.api.cestech.com.cn:48002/mission_b/v1/user/message/status`,
        header: {
          'x-Resource-Token': uni.getStorageSync('xtoken'),
          //'x-Resource-Token':uni.getStorageSync('xtoken'), //自定义请求头信息
        },
        data: {
          id: this.id,
        },
        method: 'GET',
        success: res => {
          uni.$emit('refreshList')
          // console.log(res.data.result.records,'消息')
          // this.$refs.paging.complete(res.data.result.records);
        },
      })
    },
    goBack() {
      uni.$u.route({
        type: 'navigateBack',
        delta: 1,
      })
    },
  },
}
</script>

<style lang="scss" scoped>
.mainBox {
  height: 100vh;
  width: 100%;

  .content {
    width: 100%;
    height: 100%;

    .title-box {
      border: 1px solid $u-info;
      width: 100%;
      padding: 40rpx;
      border-radius: 5px;
    }
  }
}
</style>
