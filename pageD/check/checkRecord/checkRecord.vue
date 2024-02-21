<template>
  <view>
    <mescroll-body
      ref="mescrollRef"
      @init="mescrollInit"
      @down="downCallback"
      @up="upCallback">
      <view style="height: 20rpx"></view>
      <view class="list" v-for="(item, index) in listData" :key="item.id">
        <view class="title">
          <view class="userinfo">
            <view class="avatar"><u-avatar :src="avatarUrl"></u-avatar></view>
            <view class="name">{{ nikename }}</view>
          </view>
          <view>
            <view class="status">
              <span
                style="color: #7dbd1a"
                v-if="item.reviewStatus == 'TO_BE_REVIEWED'"
                >待审核</span
              >
              <span
                style="color: #39d591"
                v-if="item.reviewStatus == 'REVIEWED'"
                >已审核（通过）</span
              >
              <span style="color: #d9001b" v-if="item.reviewStatus == 'FAILED'"
                >已审核（未通过）</span
              >
            </view>
            <view class="checkTime">{{ item.createTime }}</view>
          </view>
        </view>
        <view class="listMain">
          <view class="listMainTitle">
            {{
              item.punchCardDescribe ? item.punchCardDescribe : '暂无心得体会'
            }}
          </view>
          <view class=""> </view>
          <view class="imgList" v-if="item.punchCardArrayUrl">
            <view
              class="img"
              v-for="(imgItem, imgIndex) in item.punchCardArrayUrl.split(',')"
              :key="imgIndex">
              <image
                :src="imgItem"
                mode="scaleToFill"
                @click="() => clickImg(imgItem)"></image>
            </view>
          </view>
        </view>
        <view class="itemFooter">
          <view class="address">
            {{ item.punchCardAddress }}
          </view>
          <view
            v-if="item.reviewStatus == 'TO_BE_REVIEWED'"
            class="btn"
            @click="
              () => {
                showDelete = true
                id = item.id
              }
            ">
            删除
          </view>
        </view>

        <view class="reason" v-if="item.reviewStatus == 'FAILED'">
          审核未通过原因: {{ item.failedRemark }}
        </view>
      </view>
    </mescroll-body>

    <u-modal
      :show="showDelete"
      title="提示"
      @cancel="showDelete = false"
      @confirm="handleDelete"
      :showCancelButton="true"
      content="确定要删除该打卡记录吗?"></u-modal>
  </view>
</template>

<script>
import MescrollMixin from '@/uni_modules/mescroll-uni/components/mescroll-uni/mescroll-mixins.js'
import cellGroup from '../../../uni_modules/uview-ui/libs/config/props/cellGroup'
export default {
  mixins: [MescrollMixin], // 使用mixin
  data() {
    return {
      nikename: '',
      avatarUrl: '',
      phone: '',
      id: '',
      showDelete: false,
      listData: [],
      imgList: [],
    }
  },

  created() {
    this.phone = uni.getStorageSync('phone')
  },
  onLoad() {
    this.avatarUrl = uni.getStorageSync('avatarUrl')
    this.nikename = uni.getStorageSync('nikename')
    this.phone = uni.getStorageSync('phone')
  },
  mounted() {},
  methods: {
    clickImg(url) {
      console.log(url)
      uni.previewImage({
        urls: [url], //需要预览的图片http链接列表，多张的时候，url直接写在后面就行了
        current: '', // 当前显示图片的http链接，默认是第一个
        success: function (res) {},
        fail: function (res) {},
        complete: function (res) {},
      })
    },
    handleDelete(e) {
      uni.request({
        url: `https://wj.api.cestech.com.cn:48002/mission_b/xcx_api/bkbPunchCardRecord/deleteRecord?id=${this.id}`,
        method: 'GET',
        success: res => {
          uni.showToast({
            title: res.data.message,
            icon: 'success',
          })
          this.mescroll.resetUpScroll()
          this.showDelete = false
        },
        fail: res => {
          this.showDelete = false
        },
        finally: () => (this.showDelete = false),
      })
    },
    /*下拉刷新的回调 */
    downCallback() {
      this.mescroll.resetUpScroll()
    },
    /*上拉加载的回调: 其中page.num:当前页 从1开始, page.size:每页数据条数,默认10 */
    upCallback(page) {
      console.log('pagepage', page)
      this.phone = uni.getStorageSync('phone')
      uni.request({
        url: `https://wj.api.cestech.com.cn:48002/mission_b/xcx_api/bkbPunchCardRecord/list?pageNo=${page.num}&pageSize=10&punchCardPhone=${this.phone}`,
        method: 'GET',
        success: res => {
          // this.imgList = res.data.result.records.
          // this.listData = res.data.records
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

<style lang="scss">
page {
  background: #f5f5f5;
}

.list {
  padding: 30rpx;
  background-color: #fff;
  margin-bottom: 30rpx;

  .title {
    display: flex;
    justify-content: space-between;
    align-items: center;
    .status {
      font-size: 28rpx;
      text-align: center;
      margin-bottom: 8rpx;
    }

    .checkTime {
      font-size: 28rpx;
      color: #a6a6a6;
    }

    .userinfo {
      display: flex;
      justify-content: left;
      align-items: center;

      .avatar {
        margin-right: 30rpx;
      }

      .name {
        font-size: 28rpx;
      }
    }
  }

  .listMain {
    .listMainTitle {
      font-size: 30rpx;
      font-weight: 500;
      margin: 20rpx 0;
    }

    .imgList {
      display: flex;
      justify-content: left;

      .img {
        margin-right: 40rpx;

        image {
          height: 160rpx;
          width: 160rpx;
          border-radius: 15rpx;
        }
      }
    }
  }
}
.itemFooter {
  margin-top: 20rpx;
  display: flex;
  justify-content: space-between;
  .address {
    font-size: 28rpx;
  }
  .btn {
    font-size: 28rpx;
    width: 140rpx;
    height: 60rpx;
    line-height: 60rpx;
    background-color: #fff;
    border: #39d591 2rpx solid;
    border-radius: 14rpx;
    text-align: center;
    color: #39d591;
  }
}
.reason {
  margin-top: 10rpx;
  font-size: 28rpx;
}
</style>
