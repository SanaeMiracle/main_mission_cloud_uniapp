<template>
  <z-paging
    ref="paging"
    v-model="dataList"
    @query="queryList"
    use-page-scroll
    fixed>
    <view class="content">
      <u-cell-group>
        <u-cell
          v-for="(item, index) in dataList"
          :key="index"
          :value="item.createTime"
          @click="goDetail(item)">
          <u--text slot="title" :lines="1" :text="item.content"> </u--text>
          <view style="position: relative" slot="icon">
            <u-badge
              v-if="!item.is_read"
              :customStyle="{ 'z-index': 1 }"
              :isDot="true"
              type="error"
              absolute
              :offset="['2px', '2px']">
            </u-badge>
            <u-icon size="32" name="order"></u-icon>
          </view>
          <view slot="value">
            <u--text type="info" :text="item.createTime"> </u--text>
          </view>
        </u-cell>
      </u-cell-group>
    </view>
  </z-paging>
</template>

<script>
import ZPMixin from '@/uni_modules/z-paging/components/z-paging/js/z-paging-mixin'
import { mapState } from 'vuex'

import { messagelist } from '@/common/api.js'
export default {
  components: {},
  mixins: [ZPMixin],
  data() {
    return {
      globalData: getApp().globalData,
      dataList: [],
    }
  },
  computed: {},
  watch: {},
  onLoad(option) {
    uni.$on('refreshList', () => {
      this.$refs.paging.reload()
    })
  },
  onUnload() {
    uni.$off('refreshList')
  },
  methods: {
    queryList(page, list_rows) {
      uni.request({
        url: `https://wj.api.cestech.com.cn:48002/mission_b/v1/user/message/list`,
        header: {
          'x-Resource-Token': uni.getStorageSync('xtoken'),
          //'x-Resource-Token':uni.getStorageSync('xtoken'), //自定义请求头信息
        },
        data: {
          pageNo: page,
          pageSize: list_rows,
          // UserId:uni.getStorageSync('users').id
        },
        method: 'GET',
        success: res => {
          // console.log(res.data.result.records,'消息')
          this.$refs.paging.complete(res.data.result.records)
        },
      })

      // messagelist({
      // 	page: page,
      // 	list_rows: list_rows
      // }).then((res) => {

      // 	this.$refs.paging.complete(res.data.data);
      // }).catch(res => {
      // 	this.$refs.paging.complete(false);
      // })
    },
    goDetail(item) {
      console.log(item, '消息')
      if (!item.is_read) {
        this.$store.commit('subtractMessage', 1)
      }
      uni.$u.route({
        url: 'pageC/message/detail',
        params: {
          id: item.id,
          content: item.content,
          createTime: item.createTime,
        },
      })
      item.is_read = 1
    },
  },
}
</script>

<style lang="scss" scoped></style>
