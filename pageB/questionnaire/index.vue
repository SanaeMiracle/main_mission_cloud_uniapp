<template>
  <view>
    <view style="height: 20rpx"></view>
    <mescroll-body
      ref="mescrollRef"
      @init="mescrollInit"
      @down="downCallback"
      @up="upCallback">
      <view class="list" v-for="item in listData">
        <view class="title">{{ item.item.title }}</view>
        <view class="list-main" v-if="item.dt">
          {{ item.tm }}题&emsp;|&emsp;结束时间：{{
            item.item.endDate
          }}&emsp;|&emsp;答卷人数:{{ item.dt }}
        </view>
        <view class="list-main" v-else>
          {{ item.tm }}题&emsp;|&emsp;结束时间：{{
            item.item.endDate
          }}&emsp;|&emsp;答卷人数:0
        </view>
        <view class="btn">
          <view class="press" @click="qrcode(item.item.id)">获取二维码</view>
          <view
            class="press"
            @click="to_details(item.item.id, item.item.isArea)"
            >进入问卷</view
          >
        </view>
      </view>
    </mescroll-body>
    <view style="height: 60rpx"></view>
  </view>
</template>

<script>
import MescrollMixin from '@/uni_modules/mescroll-uni/components/mescroll-uni/mescroll-mixins.js'
export default {
  mixins: [MescrollMixin], // 使用mixin
  data() {
    return {
      listData: [],
    }
  },
  onLoad() {},
  methods: {
    qrcode(id) {
      uni.navigateTo({
        url: `/pageB/questionnaire/qrcode?id=${id}`,
      })
    },
    to_details(value, isarea) {
      uni.navigateTo({
        url: `/pageB/questionnaire/details?id=${value}&isarea=${isarea}`,
      })
    },
    /*下拉刷新的回调 */
    downCallback() {
      this.mescroll.resetUpScroll()
    },
    /*上拉加载的回调: 其中page.num:当前页 从1开始, page.size:每页数据条数,默认10 */
    upCallback(page) {
      console.log(page)
      uni.request({
        url: 'https://wj.api.cestech.com.cn:48002/mission_b/wx/bkbQuestionnaireInquiry/list',
        header: {
          'x-Resource-Token': uni.getStorageSync('xtoken'),
          //'x-Resource-Token':uni.getStorageSync('xtoken'), //自定义请求头信息
        },
        data: {
          // phone: uni.getStorageSync("phone"),
          pageNo: page.num,
          pageSize: page.size,
        },
        method: 'GET',
        success: res => {
          console.log(res.data.result, 'cc')
          this.mescroll.endSuccess(res.data.result.length)
          // this.mescroll.endSuccess(res.data.result.records.length);
          res.data.result.forEach(v => {
            // if (v.numberAnswers === null) {
            //   v.numberAnswers = 0
            // }
            console.log(v)
          })
          //设置列表数据
          if (page.num == 1) this.listData = [] //如果是第一页需手动制空列表
          this.listData = this.listData.concat(res.data.result) //追加新数据
        },
      })
      //联网加载数据
      // points_log({
      // 	page: page.num,
      // 	list_rows: page.size,
      // }).then(res => {
      // 	// console.log(res.data.data.records, 'res');

      // 	//联网成功的回调,隐藏下拉刷新和上拉加载的状态;
      // 	this.mescroll.endSuccess(res.data.data.length);

      // 	//设置列表数据
      // 	if (page.num == 1) this.listData = []; //如果是第一页需手动制空列表
      // 	this.listData = this.listData.concat(res.data.data); //追加新数据
      // 	// this.listData.forEach((item) => {
      // 	// 	console.log('item', item);
      // 	// 	// item.credits = item.credits.parseInt()
      // 	// })

      // }).catch(() => {
      // 	//联网失败, 结束加载
      // 	this.mescroll.endErr();
      // })
    },
  },
}
</script>

<style lang="scss">
page {
  background: #f5f5f5;
}

.list {
  background: #fff;
  padding: 30rpx;
  margin-bottom: 20rpx;

  .btn {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    // padding-right: 30rpx;
    margin-top: 35rpx;

    .press {
      width: 160rpx;
      font-size: 24rpx;
      background: #20c695;
      color: #fff;
      height: 60rpx;
      line-height: 60rpx;
      text-align: center;
      margin-right: 30rpx;
      border-radius: 10rpx;
    }
  }

  .title {
    width: 600rpx;
    color: #333;
    font-size: 30rpx;
    font-weight: 600;
    overflow: hidden; //超出的文本隐藏
    text-overflow: ellipsis; //溢出用省略号显示
    white-space: nowrap; // 默认不换行；
  }

  .list-main {
    font-size: 24rpx;
    color: #999;
    display: flex;
    align-items: center;
    margin-top: 30rpx;

    .btn {
      width: 120rpx;
      height: 55rpx;
      line-height: 55rpx;
      text-align: center;
      border-radius: 10rpx;
      margin-left: 50rpx;
      border: 1rpx solid #20c695;
      color: #20c695;
    }
  }
}
</style>
