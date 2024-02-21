<template>
  <view class="conetent">
    <view class="u-nav">
      <navbar
        :safeAreaInsetTop="true"
        height="55"
        placeholder
        leftIconColor="#fff"
        autoBack>
        <view class="u-nav-slot" slot="center">
          <u--text text="商品详情" color="#fff"></u--text>
        </view>
      </navbar>
    </view>
    <view style="height: 20rpx"></view>
    <view class="order-number">
      <view
        >订单编号：<text style="color: #333333">{{
          details.order.orderNumber
        }}</text>
      </view>
      <!-- <view style="display: flex;align-items: center;justify-content: flex-end;margin-left: auto;color:red ;"
				class="status">
				待发货</view> -->
      <!-- <view v-if="details.order.status!=0">订单编号：<text style="color: silver;">{{details.order.orderNumber}}</text>
			</view>
			<view v-if="details.order.status==0"
				style="display: flex;align-items: center;justify-content: flex-end;margin-left: auto;color:red ;"
				class="status">
				待发货</view> -->
      <view
        v-if="details.order.status == 1"
        style="color: #39d48f"
        class="status"
        >已发货</view
      >
      <view
        v-if="details.order.status == 2"
        style="color: silver"
        class="status"
        >已完成</view
      >
    </view>
    <view class="shop-details">
      <view>
        <image
          :src="details.orderItem.pic"
          style="width: 140rpx; height: 140rpx"
          mode="scaleToFill">
        </image>
      </view>
      <view style="margin-left: 20rpx">
        <view class="name">
          <view>{{ details.orderItem.prodName }}</view>
          <view
            v-if="details.order.status == 0"
            style="color: #ff3838"
            class="status"
            >待发货</view
          >
          <view
            v-if="details.order.status == 1"
            style="color: #20c06b"
            class="status"
            >已发货</view
          >
          <view
            v-if="details.order.status == 2"
            style="color: #999999"
            class="status"
            >已完成</view
          >
        </view>
        <view class="specifications"
          >{{ details.orderItem.skuName }}*{{ details.order.productNums }}</view
        >
      </view>
    </view>
    <view class="item">
      <view>兑换积分：</view>
      <view style="color: #333">{{ details.orderItem.allPoints }}积分</view>
    </view>
    <view class="item">
      <view>运费：</view>
      <view style="color: #333">包邮</view>
    </view>

    <view class="item">
      <view>兑换时间：</view>
      <view style="color: #333">{{ details.orderItem.recTime }}</view>
    </view>
    <view class="item" v-if="details.order.status != 0">
      <view>物流公司：</view>
      <view style="color: #333">{{ details.name }}</view>
    </view>
    <view class="item" v-if="details.order.status != 0">
      <view>物流单号：</view>
      <view style="color: #333">{{ details.order.dvyFlowId }}</view>
    </view>
    <view class="item">
      <view> 合计兑换积分： </view>
      <view style="color: #333"> {{ details.orderItem.allPoints }}积分 </view>
    </view>
    <!-- <view class="amount">合计兑换积分：{{details.orderItem.allPoints}}积分</view> -->
    <view class="customer">
      <view class="btn">联系客服</view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      orderNumber: '',
      details: {},
    }
  },
  onLoad(options) {
    if (options.orderNumber) {
      this.orderNumber = options.orderNumber
      this.shopDetails()
    }
  },
  methods: {
    //获取商品详情
    shopDetails() {
      uni.request({
        url: `https://wj.api.cestech.com.cn:48002/mission_b/p/order/pointRecordX`,
        header: {
          'x-Resource-Token': uni.getStorageSync('xtoken'),
        },
        data: {
          orderNumber: this.orderNumber,
        },

        method: 'GET',
        success: res => {
          this.details = res.data.result
        },
      })
    },
  },
}
</script>

<style lang="scss">
.u-button {
  background: #1dd193 !important;
  color: #fff !important;
}
.conetent {
  padding: 20rpx 40rpx;

  .order-number {
    font-size: 30rpx;
    display: flex;
    align-items: center;
    justify-content: space-between;

    .status {
      font-size: 34rpx;
    }
  }

  .shop-details {
    margin: 25rpx 0;
    display: flex;
    align-items: center;
    color: silver;
    font-size: 32rpx;

    .name {
      width: 510rpx;
      display: flex;
      align-items: center;
      justify-content: space-between;
      color: #333;
      font-weight: 550;
      .status {
        font-size: 30rpx !important;
      }
    }

    .specifications {
      font-size: 30rpx;
      color: #999;
      margin-top: 10rpx;
    }
  }

  .item {
    margin-top: 25rpx;
    color: #999;
    font-size: 30rpx;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .amount {
    display: flex;
    justify-content: flex-end;
    margin: 45rpx 0 60rpx 0;
    color: silver;
    font-size: 34rpx;
  }
}
.customer {
  margin-top: 60rpx;
  display: flex;
  justify-content: flex-end;
  .btn {
    text-align: center;
    line-height: 72rpx;
    width: 191rpx;
    height: 72rpx;
    background: #29ce8c;
    border-radius: 11rpx;
    font-weight: 500;
    color: #ffffff;
  }
}
</style>
