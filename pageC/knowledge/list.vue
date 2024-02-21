<template>
  <view>
    <locationNavbar :title="title" maxWidth="250rpx" />
    <z-paging
      ref="paging"
      v-model="dataList"
      @query="queryList"
      use-page-scroll>
      <view class="content u-p">
        <knowledgeCard
          v-for="(item, index) in dataList"
          :key="index"
          :item="item.bkbKnowledges"
          @click.native="goknowledgedetail(item)">
          <view>44</view>
        </knowledgeCard>
      </view>
    </z-paging>
  </view>
</template>

<script>
import ZPMixin from '@/uni_modules/z-paging/components/z-paging/js/z-paging-mixin'
import { mapState } from 'vuex'
import knowledgeCard from './components/knowledge-card.vue'
import locationNavbar from '@/pages/index/components/location-navbar.vue'

import { culturalistbycustomer, view_click } from '@/common/api.js'
export default {
  components: {
    knowledgeCard,
    locationNavbar,
  },
  mixins: [ZPMixin],
  data() {
    return {
      type: '',
      title: '',
      dataList: [],
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
  watch: {
    customer(val) {
      console.log(val, '我是val')
      this.$refs.paging.reload()
    },
  },
  onShow() {
    // this.queryList();
  },
  onLoad(option) {
    // this.queryList()
    // console.log(1111111111111111)
    if (option.title) {
      this.title = option.title
      console.log(1111111111111111)
    } else {
      this.title = '文化知识列表'
      console.log(22222)
    }
    if (option.type) {
      this.type = option.type
    } else {
      uni.$u.route({
        type: 'navigateBack',
      })
      console.log(3333)
      return
    }
    console.log(444)

    // culturalistbycustomer({

    // 	type: this.type,
    // 	page: 1,
    // 	list_rows: 10
    // }).then(res=>{
    // 	console.log('我是res',res)
    // })
    console.log(555)
  },

  methods: {
    queryList(page, list_rows) {
      uni.request({
        // url: 'https://wj.api.cestech.com.cn:48002/mission_b/v1/sy/cultura/listbycustomer',
        url: 'https://wj.api.cestech.com.cn:48002/mission_b/v1/sy/cultura/listbycustomer',
        header: {
          'x-Resource-Token': uni.getStorageSync('xtoken'),
          //'x-Resource-Token':uni.getStorageSync('xtoken'), //自定义请求头信息
        },
        data: {
          pageNo: page,
          pageSize: list_rows,
          type: this.type,
        },
        method: 'GET',
        success: res => {
          console.log(res.data.result.records, 'xxxx')
          res.data.result.records.forEach(v => {
            // console.log(v, 'vvvv')
            v.bkbKnowledges[0]['points'] = v.bkbCulturalKnowledge.points
            v.bkbKnowledges[0]['status'] = v.view_count
            // console.log(v,'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
          })
          this.$refs.paging.complete(res.data.result.records)
          console.log(res.data.result.records, 'res.data.data')
        },
      })
    },
    goknowledgedetail(item) {
      // this.view_click(item)
      console.log(item, '000')
      uni.$u.route({
        url: 'pageC/knowledge/detail',
        params: {
          id: item.bkbCulturalKnowledge.knowledgeId,
          points: item.bkbCulturalKnowledge.points,
          title: item.bkbCulturalKnowledge.knowledgeTitle,
          source_id: item.bkbCulturalKnowledge.id,
          // source_type: item.points_source_type,
          points_acquisition_rule:
            item.bkbCulturalKnowledge.pointsAcquisitionRule,
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

<style lang="scss" scoped></style>
