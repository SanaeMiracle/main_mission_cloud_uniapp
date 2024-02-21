<template>
  <view>
    <locationNavbar title="活动管理" maxWidth="250rpx" />
    <view class="tabbar">
      <!--      <uni-segmented-control :current="subsectionCurrent" :values="subsectionList"
		    :active-color="globalData.theme['$u-primary']" @clickItem="subsectionChange" /> -->
      <!-- :activeStyle="{'font-size': '28rpx' ,' font-weight': 'bold' , color: '#333333'  }" :inactiveStyle="{ 'font-size': '24rpx' , 'font-weight': 'bold' , color: '#666666'  }" -->
      <u-tabs
        lineColor="#39d48f"
        :scrollable="false"
        :list="subsectionTabs"
        lineWidth="30"
        keyName="name"
        @click="subsectionChange"></u-tabs>
    </view>
    <z-paging
      ref="paging"
      v-model="dataList"
      @query="queryList"
      use-page-scroll>
      <view class="content u-p-l u-p-r">
        <activityCard
          v-for="(item, index) in dataList"
          :key="index"
          :item.sync="item"
          :index="index"
          @changeBooked="changeBooked">
        </activityCard>
      </view>
    </z-paging>
  </view>
</template>

<script>
import { actStatus } from '@/common/enums/activity.js'
import ZPMixin from '@/uni_modules/z-paging/components/z-paging/js/z-paging-mixin'
import { mapState } from 'vuex'
import activityCard from './components/activity-card.vue'
import locationNavbar from '@/pages/index/components/location-navbar.vue'

import { activitylist } from '@/common/api.js'
export default {
  components: {
    activityCard,
    locationNavbar,
  },
  mixins: [ZPMixin],
  data() {
    return {
      globalData: getApp().globalData,
      dataList: [],
      subsectionList: ['待开始', '进行中', '已结束'],
      subsectionTabs: [
        {
          name: '待开始',
        },
        {
          name: '进行中',
        },
        {
          name: '已结束',
        },
      ],
      subsectionCurrent: 0,
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
    status() {
      if (this.subsectionCurrent == 0) {
        return actStatus.popular
      } else if (this.subsectionCurrent == 1) {
        return actStatus.underway
      }
      return actStatus.success
    },
  },
  watch: {
    customer(val) {
      this.$refs.paging.reload()
    },
  },
  onLoad(option) {},

  methods: {
    subsectionChange(e) {
      if (this.current != e.index) {
        this.subsectionCurrent = e.index
        this.$refs.paging.reload()
      }
    },
    changeBooked({ index, key, value }) {
      this.$set(this.dataList[index], key, value)
    },
    queryList(page, list_rows) {
      // if (!this.customer) {
      // 	this.$refs.paging.complete([]);
      // 	return;
      // }
      const userId = uni.getStorageSync('users').id
      uni.request({
        url: 'https://wj.api.cestech.com.cn:48002/mission_b/v1/activity/list',
        header: {
          'x-Resource-Token': uni.getStorageSync('xtoken'),
          //'x-Resource-Token':uni.getStorageSync('xtoken'), //自定义请求头信息
        },
        data: {
          pageNo: page,
          pageSize: list_rows,
          status: this.status,
          customer_id: 1,
        },
        method: 'GET',
        success: res => {
          console.log(res.data.result, 'xxxx1')
          res.data.result.records.forEach(item => {
            uni.request({
              // url: `https://wj.api.cestech.com.cn:48002/mission_b/v1/2/book/subscribe?user_id=${userId}&bookId=${item.id}`,
              url: `https://wj.api.cestech.com.cn:48002/mission_b/v1/2/book/subscribe?bookId=${item.id}`,
              header: {
                'x-Resource-Token': uni.getStorageSync('xtoken'),
                //'x-Resource-Token':uni.getStorageSync('xtoken'), //自定义请求头信息
              },
              method: 'POST',
              success: res => {
                item.booked = res.data
                console.log(res)
              },
            })
          })

          this.$refs.paging.complete(res.data.result.records)
        },
      })
      // activitylist({
      // 	customer_id: 1,
      // 	status: this.status,
      // 	page: page,
      // 	list_rows: list_rows
      // }).then((res) => {

      // 	this.$refs.paging.complete(res.data.data);
      // }).catch(res => {
      // 	console.log(res)
      // 	//如果请求失败写this.$refs.paging.complete(false)，会自动展示错误页面
      // 	//注意，每次都需要在catch中写这句话很麻烦，z-paging提供了方案可以全局统一处理
      // 	//在底层的网络请求抛出异常时，写uni.$emit('z-paging-error-emit');即可
      // 	this.$refs.paging.complete(false);
      // })
    },
    goActivitydetail(item) {
      uni.$u.route({
        url: 'pageC/knowledge/detail',
        params: {
          id: item.knowledge_id,
        },
      })
    },
    goLocation() {
      uni.navigateTo({
        url: '/pages/index/city',
      })
    },
  },
}
</script>

<style lang="scss" scoped>
.tabbar {
  background: #fff;
  height: 100rpx;
}
</style>
