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
          <u--text text="分类查询" color="#fff"></u--text>
        </view>
      </navbar>
    </view>

    <view class="search">
      <u--input
        placeholder="请输入内容"
        border="surround"
        v-model="value"
        shape="circle"
        @change="
          () => {
            ;(showList = false), (showTips = false)
          }
        "></u--input>
      <!-- <input placeholder="请输入内容" border="surround" v-model="value" shape='circle' @change='handleSearch'></input> -->
      <view class="btn" @click="showList = true">搜索</view>
    </view>

    <view class="category" v-if="!showList && !showTips">
      <view class="banner">
        <view class="img">
          <image
            @click="showItem = 1"
            class="image"
            src="https://oms.cestech.com.cn/BkpKnowledge/img/edb4608e6b1f43e7a94162619865fd89/chuyulaji.png"
            mode="aspectFit" />
        </view>
        <view class="img">
          <image
            @click="showItem = 2"
            class="image"
            src="https://oms.cestech.com.cn/BkpKnowledge/img/10bcd58a02d64fda91ab413acc5200fb/kehuishouwu.png"
            mode="aspectFit" />
        </view>
        <view class="img">
          <image
            @click="showItem = 3"
            class="image"
            src="https://oms.cestech.com.cn/BkpKnowledge/img/fb019dad8f174c75bf11dc262468994a/youhailaji.png"
            mode="aspectFit" />
        </view>
        <view class="img">
          <image
            @click="showItem = 4"
            class="image"
            src="https://oms.cestech.com.cn/BkpKnowledge/img/badebe1a7ced4a1f83bbf509fc1f15e8/qitalaji.png"
            mode="aspectFit" />
        </view>
      </view>
      <view class="triangleList">
        <view
          class="triangle"
          style="left: 60rpx"
          v-show="showItem == 1"></view>
        <view
          class="triangle"
          style="left: 220rpx"
          v-show="showItem == 2"></view>
        <view
          class="triangle"
          style="left: 400rpx"
          v-show="showItem == 3"></view>
        <view
          class="triangle"
          style="left: 560rpx"
          v-show="showItem == 4"></view>
      </view>
      <view class="contentList">
        <view class="content" v-if="showItem == 1">
          主要为家庭生活消耗产生的蔬菜瓜果皮、腐肉、碎骨、蛋壳、禽畜内脏等，具有高度易腐的特性。统一用绿色垃圾桶收纳。
        </view>
        <view class="content" v-else-if="showItem == 2">
          废纸、废塑料、废金属、废包装物、废旧纺织物、废弃电器电子产品、废玻璃、废纸塑铝复合包装等。可回收物需未受有毒有害物质污染，未受血污、油污污染，用蓝色垃圾桶收纳。</view
        >
        <view class="content" v-else-if="showItem == 3">
          废电池（镉镍电池、氧化汞电池、铅蓄电池等），废荧光灯管（日光灯管、节能灯等），废温度计，废血压计，废药品及其包装物，废油漆、溶剂及其包装物，废杀虫剂、消毒剂及其包装物，废胶片及废相纸等。其中含有重金属类的电池、荧光灯管、杀虫剂、温度计、血压计等应保证包装完整，投入指定投放点收集容器；所有过期药品、保健品连同其包装物投放至指定过期药品回收点。统一由红色垃圾桶收纳。</view
        >
        <view class="content" v-else-if="showItem == 4">
          主要包括受污染与无法再生的纸张（纸杯、照片、复写纸、压敏纸、收据用纸、明信片、相册、卫生纸、尿片等）；受污染或其他不可回收的玻璃、塑料袋与其他受污染的塑料制品；不可回收的废旧衣物与其他纺织品；破旧陶瓷品；妇女卫生用品；一次性餐具、烟头、灰土等。</view
        >
      </view>
    </view>

    <view class="footer" v-if="showList">
      <mescroll-body
        ref="mescrollRef"
        @init="mescrollInit"
        @down="downCallback"
        @up="upCallback">
        <view v-for="item in listData" :key="item.id">
          <view class="title">{{ item.name }}</view>
          <view class="result">{{ item.type }}</view>
          <u-divider></u-divider>
        </view>
      </mescroll-body>
    </view>

    <view v-if="showTips" class="tips">暂未找到对应垃圾类型</view>
  </view>
</template>

<script>
import MescrollMixin from '@/uni_modules/mescroll-uni/components/mescroll-uni/mescroll-mixins.js'
export default {
  mixins: [MescrollMixin], // 使用mixin
  data() {
    return {
      value: '',
      showItem: 0,
      result: [],
      listData: [],
      showList: false,
      showTips: false,
    }
  },
  methods: {
    handleSearch() {
      uni.request({
        url: `https://wj.api.cestech.com.cn:48002/mission_b/mission/bkbClassifyMain/list?name=${this.value}`,
        method: 'GET',
        success: res => {
          if (res.data.result.records.length <= 0)
            this.result = [{ name: this.value, type: '未知' }]
          else this.result = res.data.result.records
        },
        fail: res => {},
      })
    },
    /*下拉刷新的回调 */
    downCallback() {
      this.mescroll.resetUpScroll()
    },
    /*上拉加载的回调: 其中page.num:当前页 从1开始, page.size:每页数据条数,默认10 */
    upCallback(page) {
      uni.request({
        url: `https://wj.api.cestech.com.cn:48002/mission_b/mission/bkbClassifyMain/list?name=${this.value}&pageNo=${page.num}`,
        method: 'GET',
        success: res => {
          if (res.data.result.records.length <= 0 && page.num == 1) {
            this.showList = false
            this.showTips = true
          }
          this.mescroll.endSuccess(res.data.result.records.length)

          //设置列表数据
          if (page.num == 1) this.listData = [] //如果是第一页需手动制空列表
          this.listData = this.listData.concat(res.data.result.records) //追加新数据
        },
        fail: res => {
          this.mescroll.endErr()
        },
      })
    },
  },
}
</script>

<style lang="scss" scoped>
.search {
  width: 90%;
  margin: 40rpx auto;
  display: flex;
  justify-content: left;

  .btn {
    width: 140rpx;
    border-radius: 40rpx;
    background-color: #59ce97;
    text-align: center;
    color: #fff;
    line-height: 76rpx;
    margin-left: 8rpx;
    font-size: 28rpx;
  }
}

.category {
  width: 90%;
  margin: 0 auto;
  /* margin-top: -100rpx; */
  .banner {
    display: flex;
    justify-content: space-around;
    justify-items: center;
    height: auto;
    .image {
      width: 160rpx;
      height: 240rpx;
    }
  }
}
.triangleList {
  /* display: flex;
  justify-content: space-around;
  justify-items: center; */
  /* margin-top: -130rpx; */
  .triangle {
    width: 0;
    height: 0;
    border-width: 30rpx; /* 设置边长 */
    /* border-style: solid; */
    /* background-color: #e4e6ee; */
    border-bottom: 30rpx solid #e4e6ee;
    border-right: 30rpx solid transparent;
    border-left: 30rpx solid transparent;
    position: relative;
  }
}
.contentList {
  /* width: 90%; */
  .content {
    background: #e4e6ee;
    padding: 40rpx;
    border-radius: 10rpx;
  }
}
.footer {
  margin: 60rpx auto;
  text-align: center;
  .title {
    margin: 0 auto;
    text-align: center;
    font-size: 28rpx;
  }
  .result {
    text-align: center;
    margin: 40rpx auto;
    color: #0070ea;
    font-size: 36rpx;
    font-weight: 700;
  }
}
.tips {
  font-size: 36rpx;
  font-weight: 700;
  text-align: center;
  margin-top: 200rpx;
}
</style>
