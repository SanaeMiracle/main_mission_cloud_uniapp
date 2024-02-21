<template>
  <view>
    <view class="u-nav">
      <navbar :safeAreaInsetTop="true" height="55" placeholder>
        <view class="u-nav-slot" slot="left">
          <u--text text="垃圾分类宣教云" color="#fff"></u--text>
        </view>
      </navbar>
    </view>
    <view class="conetent-body">
      <u-swiper
        :list="swiperList"
        keyName="url"
        previousMargin="30"
        nextMargin="30"
        circular
        radius="5"
        @click="swiperClick"
        bgColor="#ffffff"
        @change="swiperChange">
      </u-swiper>
      <view class="indicators u-flex">
        <view
          class="indicators__dot"
          v-for="(item, index) in swiperList"
          :key="index"
          :class="[index === current && 'indicators__dot--active']">
        </view>
      </view>
      <u-gap bgColor="transparent" height="15"></u-gap>
      <u-grid :border="false" col="6">
        <u-grid-item
          v-for="(listItem, listIndex) in menuList"
          :key="listIndex"
          @click.stop="gridClick(listItem, listIndex)">
          <u--image
            mode="aspectFit"
            :showLoading="true"
            :src="listItem.url"
            width="110px"
            height="60px">
          </u--image>

          <u--text align="center" bold :text="listItem.title"></u--text>
        </u-grid-item>
      </u-grid>
      <!-- <u-grid :border="false" col="3">
				<u-grid-item v-for="(listItem,listIndex) in menuList" :key="listIndex"
					@click.stop="gridClick(listItem,listIndex)">

					<u--image mode="aspectFit" :showLoading="true" :src="listItem.url" width="200px" height="80px">
					</u--image>

					<u--text align="center" bold :text="listItem.title"></u--text>
				</u-grid-item>
			</u-grid> -->
      <!-- 	<u-grid :border="false" col="3">
				<u-grid-item v-for="(listItem,listIndex) in menuList" :key="listIndex"
					@click.stop="gridClick(listItem,listIndex)">
			
					<u--image mode="aspectFit" :showLoading="true" :src="listItem.url" width="200px" height="80px">
					</u--image>
			
					<u--text align="center" bold :text="listItem.title"></u--text>
				</u-grid-item>
			</u-grid> -->
      <!-- <view class="application-list">
				
				<view class="li" @click="activity_customized">
					<view><image src="@/static/assets/home/dz.png" style="width: 50px;height: 50px;" mode="aspectFill"></image></view>
					<view style="margin-top: 10rpx;">活动定制</view>
				</view>
				<view class="li" @click="teacher_reservation">
					<view><image src="@/static/assets/home/yy.png" style="width: 50px;height: 50px;" mode="aspectFill"></image></view>
					<view style="margin-top: 10rpx;">预约老师</view>
				</view>
				<view class="li" @click="my_reservation">
					<view><image src="@/static/assets/home/myyy.png" style="width: 56px;height: 56px;" mode="aspectFill"></image></view>
					<view style="margin-top: 10rpx;">我要预约</view>
				</view>
				
				
			</view> -->
      <view class="application-bottom">
        <view class="li" @click="activity_customized">
          <view>
            <image
              src="http://cdn.cestech.com.cn/BkpKnowledge/img/9ddfbdf38be147cbb1c05ffb415e1ecc.png"
              style="width: 338.27rpx; height: 199.73rpx"
              mode="aspectFill"></image>
          </view>
        </view>
        <view class="li" @click="teacher_reservation">
          <view>
            <image
              src="http://cdn.cestech.com.cn/BkpKnowledge/img/3b4a4eec22f54e5298a784a44fda1353.png"
              style="width: 338.27rpx; height: 199.73rpx"
              mode="aspectFill"></image>
          </view>
        </view>
      </view>
      <view class="u-flex u-p kc">
        <view class="u-flex-1 kc-left">
          <image
            @click="goCourse"
            style="width: 100%; height: 100%"
            mode="aspectFit"
            src="http://cdn.cestech.com.cn/static/door.png">
          </image>
        </view>
        <view class="u-flex-1 u-m-l kc-right u-flex-col">
          <view
            class="u-flex-col u-flex-1 kc-right__item"
            v-for="(item, index) in list1"
            :key="item.id"
            @click="goknowledgedetail(item)">
            <view class="z-card">
              <view class="z-card__title">
                <u--text lines="2" bold :text="item.title"></u--text>
              </view>

              <view class="z-card__info z-card__info-ss">
                <view class="z-card__info--left">
                  <u--text
                    size="13"
                    prefixIcon="/static/assets/home/hot.png"
                    lines="1"
                    type="info"
                    :iconStyle="{
                      'margin-right': '5px',
                    }"
                    :text="item.type"></u--text>
                </view>
                <view class="z-card__jf">
                  <view class="z-card__jf-left"
                    ><image
                      style="width: 40rpx; height: 40rpx; margin-right: 8rpx"
                      src="@/static/assets/center/ck.png"
                      mode="scaleToFill"></image>
                    9999+</view
                  >
                  <!-- <view class="z-card__jf-right"><image style="width: 28rpx;height: 28rpx;margin-right: 8rpx;" src="@/static/assets/center/5.png" mode="scaleToFill"></image> 20000</view> -->
                </view>
                <!-- <view class="z-card__info--right">
                                    <u--text size="13" lines="1" type="info" text="3天前"></u--text>
                                </view> -->
              </view>
            </view>
          </view>
        </view>
      </view>

      <view
        class="z-card2"
        v-for="(item, index) in list2"
        :key="item.id"
        @click="goknowledgedetail(item)">
        <view class="z-card__content">
          <view class="z-card__info">
            <view class="z-card__info--title">
              <u--text :lines="2" bold :text="item.title || ''"></u--text>
            </view>
            <view class="z-card__info--desc">
              <view class="z-card__info--desc--left">
                <u--text
                  size="13"
                  lines="1"
                  type="info"
                  :text="item.type"></u--text>
              </view>
              <view class="integral">
                <view class="integral-left"
                  ><image
                    style="width: 40rpx; height: 40rpx; margin-right: 8rpx"
                    src="@/static/assets/center/ck.png"
                    mode="scaleToFill"></image>
                  9999+</view
                >
                <!-- <view class="integral-right" style="margin-left: 25rpx;"><image style="width: 28rpx;height: 28rpx;margin-right: 8rpx;" src="@/static/assets/center/5.png" mode="scaleToFill"></image> 20000</view> -->
              </view>
              <!-- <view class="z-card__info--desc--right">
                                <u--text size="13" lines="1" type="info" :text="item.create_time|dateFormat"></u--text>
                            </view> -->
            </view>
          </view>
          <view class="z-card__image">
            <u--image
              :showLoading="true"
              :src="item.url"
              mode="aspectFit"
              width="100%"
              height="80px"
              radius="8">
            </u--image>
          </view>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
import ZPMixin from '@/uni_modules/z-paging/components/z-paging/js/z-paging-mixin'
import { mapState } from 'vuex'
import knowledgeCard from './components/knowledge-card.vue'
import locationNavbar from './components/location-navbar.vue'
import navbar from '@/components/navbar.vue'
import { culturalistbycustomer, homeknowledge } from '@/common/api.js'
export default {
  components: {
    navbar,
    knowledgeCard,
    locationNavbar,
  },
  mixins: [ZPMixin],
  data() {
    return {
      staticUrl: getApp().globalData.staticUrl,
      menuList: getApp().globalData.index.menuList,
      current: 0,
      swiperList: getApp().globalData.index.swiperList,
      list1: getApp().globalData.index.list1,
      list2: getApp().globalData.index.list2,
    }
  },
  computed: {
    ...mapState({
      location: state => state.location,
      customer: state => state.customer,
    }),

    region() {
      if (this.location) {
        return this.location.region
      }
      return ''
    },
  },
  watch: {},
  onLoad() {},
  methods: {
    questionnaire() {
      uni.navigateTo({
        url: '/pageB/questionnaire/index',
      })
    },
    activity_customized() {
      uni.navigateTo({
        url: '/pageB/activitycustomized/index',
      })
    },
    teacher_reservation() {
      uni.navigateTo({
        url: '/pageB/appointmenteacher/index',
      })
    },
    my_reservation() {
      uni.navigateTo({
        url: '/pageB/myreservation/index',
      })
    },
    swiperChange(index) {
      this.current = index.current
    },

    gridClick(item, index) {
      if (item?.route) {
        uni.$u.route({
          url: item.route,
        })
        return
      }
      if (item.children.length > 0) {
        uni.$u.route({
          url: 'pageC/knowledge/index',
          params: {
            index: index,
          },
        })
      } else {
        uni.$u.route({
          url: 'pageC/knowledge/list',
          params: {
            type: item.id,
            title: item.title,
          },
        })
      }
    },
    swiperClick(index) {
      uni.$u.route({
        url: 'pageC/knowledge/detail',
        params: {
          id: this.swiperList[index].id,
        },
      })
    },
    teacher_reservation() {
      uni.navigateTo({
        url: '/pageB/appointmenteacher/index',
      })
    },
    activity_customized() {
      uni.navigateTo({
        url: '/pageB/activitycustomized/index',
      })
    },
    goknowledgedetail(item) {
      uni.$u.route({
        url: 'pageC/knowledge/detail',
        params: {
          id: item.id,
        },
      })
    },
    goCourse() {
      uni.$u.route({
        url: 'pageC/course/list',
      })
    },
  },
}
</script>

<style lang="scss" scoped>
.application-list {
  width: 750rpx;
  height: 160rpx;
  background: #fff;
  margin-top: 30rpx;
  margin-bottom: 20rpx;
  display: flex;
  align-items: center;
  justify-content: space-between;
  .li {
    width: 33.33%;
    text-align: center;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-decoration: none;
    font-weight: bold;
    word-wrap: normal;
    font-size: 30rpx;
    color: #303133;
  }
}
.z-card__info-ss {
  display: flex;
  align-items: center;
}
.kc {
  .kc-left,
  .kc-right {
    height: calc(60vw - var(--default-padding));

    .kc-right__item:nth-child(2) {
      margin-top: 10px;
    }
  }
}

/deep/.u-grid-item {
  overflow: hidden;
}
.z-card__info--desc {
  display: flex;
  align-items: center;
  justify-content: space-between;
  .integral {
    display: flex;
    align-items: center;
    font-size: 24rpx;
    .integral-left,
    .integral-right {
      display: flex;
      align-items: center;
    }
  }
}
.z-card__jf {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 24rpx;
  margin: 6rpx 0;
  .z-card__jf-left,
  .z-card__jf-right {
    display: flex;
    align-items: center;
  }
  .z-card__jf-right {
  }
}
.application-bottom {
  display: flex;
  justify-content: space-around;
}
</style>
