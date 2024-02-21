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
          <u--text text="我的积分" color="#fff"></u--text>
        </view>
      </navbar>
    </view>
    <view class="u-demo-block">
      <view class="u-demo-block__content">
        <u-sticky bgColor="#fff">
          <u-tabs
            lineColor="#39d48f"
            lineWidth="80rpx"
            @click="click"
            :list="list6"
            :current="current"
            :scrollable="false"
            :activeStyle="{
              color: '#39d48f',
              fontWeight: 'bold',
              transform: 'scale(1.05)',
            }"
            :inactiveStyle="{
              color: '#333',
              transform: 'scale(1)',
            }">
          </u-tabs>
        </u-sticky>
      </view>
    </view>
    <!-- 积分明细 -->
    <mescroll-body
      ref="mescrollRef"
      @init="mescrollInit"
      @down="downCallback"
      @up="upCallback"
      v-if="switchs === 0">
      <view style="height: 20rpx"></view>
      <view class="list" v-for="(item, index) in listData" :key="index">
        <view class="list-left">
          <view class="name">{{ item.title }}</view>
          <view class="time">{{ item.createTime }}</view>
        </view>
        <view class="list-right" v-if="item.type == 0" style="color: #29ce8c"
          >+{{ item.points }}</view
        >
        <view class="list-right" v-if="item.type == 1" style="color: #ff3838"
          >-{{ item.points }}</view
        >
      </view>
    </mescroll-body>
    <!-- 积分收支 -->
    <view v-if="switchs === 1">
      <view style="height: 15rpx"></view>
      <view
        class="income"
        v-for="item in shoplist"
        @click="shop_details(item.orderItem.orderNumber)">
        <view class="income-left">
          <view>
            <image
              :src="item.orderItem.pic"
              mode="scaleToFill"
              style="width: 140rpx; height: 140rpx">
            </image>
          </view>
          <view class="shop-name">
            <view
              class="prodname"
              style="color: #333; font-size: 32rpx; font-weight: 550">
              <view>{{ item.orderItem.prodName }}</view>
              <view class="income-right">
                <view
                  v-if="item.order.status === 0"
                  class="status"
                  style="color: #ff3838"
                  >待发货</view
                >
                <view
                  v-if="item.order.status === 1"
                  class="status"
                  style="color: #20c06b"
                  >已发货</view
                >
                <view
                  v-if="item.order.status === 2"
                  class="status"
                  style="color: #999999"
                  >已完成</view
                >
              </view>
            </view>
            <view style="margin: 7rpx 0"
              >{{ item.orderItem.skuName }}*{{ item.order.productNums }}</view
            >
            <view class="dateTime">
              <view>{{ item.orderItem.recTime }}</view>
              <view>-{{ item.bkbOperatingRecords.points }}</view>
            </view>
          </view>
        </view>

        <!-- <view class="income-right">
					<view v-if="item.order.status===0" class="status" style="color: red;">待发货</view>
					<view v-if="item.order.status===1" class="status" style="color: #39d48f;">已发货</view>
					<view v-if="item.order.status===2" class="status" style="color: silver;">已完成</view>
					<view class="jf">-{{item.bkbOperatingRecords.points}}</view>
				</view> -->
      </view>
    </view>
    <!-- 积分规则 -->
    <view v-if="switchs === 2">
      <view class="txt">
        一、积分获取规则<br />
        1、在线学习得积分。参与分类基础、行业资讯、专题报告模块学习生活垃圾分类相关知识可获得相应积分。<br />
        2、学习答题得积分。进入课程体系，完成相关学习内容或完成挑战答题，可获得相应积分。<br />

        二、积分使用规则<br />
        1、可在积分商城使用积分兑换相应商品，兑换后不可退换商品且不退返积分。<br />
        最终解释权归中环服成都科技有限公司所有。<br />
      </view>
    </view>
  </view>
</template>

<script>
import MescrollMixin from '@/uni_modules/mescroll-uni/components/mescroll-uni/mescroll-mixins.js'
export default {
  mixins: [MescrollMixin], // 使用mixin
  data() {
    return {
      current: 0,
      listData: [],
      switchs: 0,
      shoplist: [],
      list6: [
        {
          name: '积分收支',
        },
        {
          name: '积分记录',
        },
        {
          name: '积分规则',
        },
      ],
    }
  },

  created() {},
  onLoad(options) {
    if (options.switchs) {
      this.current = 1
      this.pointRecord()
    }
    if (this.current === 0) {
      this.switchs = 0
    } else if (this.current === 1) {
      this.switchs = 1
    } else if (this.current === 2) {
      this.switchs = 2
    }
  },
  mounted() {},
  methods: {
    //积分收支
    shop_details(value) {
      uni.navigateTo({
        url: `/pageA/integral/details?orderNumber=${value}`,
      })
    },
    //切换状态
    click(item) {
      this.switchs = item.index
      if (item.index === 1) {
        this.pointRecord()
      }
    },
    //积分记录列表
    pointRecord() {
      uni.request({
        url: `https://wj.api.cestech.com.cn:48002/mission_b/p/order/pointRecord`,
        header: {
          'x-Resource-Token': uni.getStorageSync('xtoken'),
        },
        method: 'GET',
        success: res => {
          this.shoplist = res.data.result
        },
      })
    },
    /*下拉刷新的回调 */
    downCallback() {
      this.mescroll.resetUpScroll()
    },
    /*上拉加载的回调: 其中page.num:当前页 从1开始, page.size:每页数据条数,默认10 */
    upCallback(page) {
      const userid = uni.getStorageSync('users').id

      uni.request({
        url: `https://wj.api.cestech.com.cn:48002/mission_b/v1/user/points_log?pageNo=${page.num}&pageSize=${page.size}`,
        header: {
          'x-Resource-Token': uni.getStorageSync('xtoken'),
        },
        method: 'GET',
        success: res => {
          //联网成功的回调,隐藏下拉刷新和上拉加载的状态;
          this.mescroll.endSuccess(res.data.result.records.length)
          //设置列表数据
          if (page.num == 1) this.listData = [] //如果是第一页需手动制空列表
          this.listData = this.listData.concat(res.data.result.records) //
        },
        complete: () => {
          this.mescroll.endErr()
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

.txt {
  border-radius: 20rpx;
  height: 1000rpx;
  width: 94%;
  background: #fff;
  margin: 20rpx auto;
  font-size: 28rpx;
  line-height: 60rpx;
  padding: 20rpx 40rpx 0 40rpx;
  font-weight: 550;
}

.income {
  border-radius: 15rpx;
  width: 725rpx;
  margin: 0 auto;
  background: #fff;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20rpx 35rpx;
  margin-bottom: 15rpx;

  .income-left {
    display: flex;
    align-items: center;
    font-size: 28rpx;
    color: #999999;

    .shop-name {
      width: 490rpx;
      margin-left: 20rpx;
      .dateTime {
        display: flex;
        align-items: center;
        justify-content: space-between;
      }
      .prodname {
        // width: 280rpx;
        // overflow: hidden; //超出的文本隐藏
        // text-overflow: ellipsis; //溢出用省略号显示
        // white-space: nowrap; // 默认不换行；
        display: flex;
        align-items: center;
        justify-content: space-between;
        .income-right {
          display: flex;
          flex-direction: column;
          align-items: center;
          color: #999999;
          font-size: 32rpx;

          .status {
            font-weight: bold;
            font-size: 30rpx;
          }

          .jf {
            font-size: 28rpx;
          }
        }
      }
    }
  }

  .income-right {
    display: flex;
    flex-direction: column;
    align-items: center;
    color: silver;
    font-size: 32rpx;

    .status {
      font-size: 34rpx;
      font-weight: 550;
    }

    .jf {
      margin-top: 10rpx;
    }
  }
}

.u-demo-block__content {
  background: #fff;
  padding-bottom: 20rpx;
}

.list {
  width: 725rpx;
  margin: 0 auto;
  border-radius: 15rpx;
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  padding: 30rpx 45rpx;
  background: #fff;
  margin-bottom: 15rpx;

  .list-left {
    .name {
      font-size: 36rpx;
      font-weight: 600;
      width: 300rpx;
      overflow: hidden; //超出的文本隐藏
      text-overflow: ellipsis; //溢出用省略号显示
      white-space: nowrap; // 默认不换行；
    }

    .time {
      font-size: 26rpx;
      color: #999;
      margin-top: 15rpx;
    }
  }

  .list-right {
    color: blue;
    // font-weight: bold;
  }
}
</style>
