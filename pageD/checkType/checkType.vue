<template>
  <view>
    <view style="height: 30rpx"></view>
    <view class="min">
      <view class="main">
        <view class="conetent-body">
          <view class="index-list">
            <view
              class="item"
              v-for="(item, index) in recoveryList"
              :key="index">
              <view class="item-left">
                <view>
                  <image
                    :src="item.url"
                    style="width: 100rpx; height: 100rpx"></image>
                </view>
                <view style="margin-left: 20rpx">
                  <view class="title">{{ item.title }}</view>
                  <view class="details" style="font-size: 30rpx">{{
                    item.tip
                  }}</view>
                </view>
              </view>
              <view>
                <!-- <view v-if="loginBtn" class="btn" @click="go_clock(item)">{{
                  item.btn
                }}</view>
                <view v-else class="btn" @click="go_clock(item)">{{
                  item.btn
                }}</view> -->
                <u-button
                  v-if="loginBtn"
                  type="primary"
                  shape="circle"
                  :text="item.btn"
                  @getphonenumber="getPhoneNumber($event, item)"
                  open-type="getPhoneNumber">
                </u-button>
                <u-button
                  v-else
                  type="primary"
                  shape="circle"
                  :text="item.btn"
                  @click="go_clock(item)">
                </u-button>
              </view>
            </view>
          </view>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
var staticUrl = 'http://cdn.cestech.com.cn/'
export default {
  data() {
    return {
      current: 0,
      recoveryList: [
        {
          title: '21天打卡',
          tip: '连续21天打卡获得大礼包',
          url: 'https://oms.cestech.com.cn/BkpKnowledge/img/82206731a0824ff782253ab1f876d39b/2.png',
          btn: '立即打卡',
          id: '1656547545604911105',
        },
        {
          title: '有害垃圾回收',
          tip: '如废电池、废日光灯管、废水银温度计、过期药品等均属于有害垃圾',
          url: 'https://oms.cestech.com.cn/BkpKnowledge/img/82206731a0824ff782253ab1f876d39b/2.png',
          btn: '立即回收',
          id: '1656523212604432104',
        },
        {
          title: '可回收物回收',
          tip: '如废纸、塑料、玻璃、金属和纺织物等均属于可回收物',
          url: 'https://oms.cestech.com.cn/BkpKnowledge/img/8ea55be2981e4ca49de06854d3a0b2a7/1.png',
          btn: '立即回收',
          id: '1656523212602311122',
        },
        {
          title: '光盘行动',
          tip: '每日早中晚，7.00-10.00、11.00-13.00、17.00-20.00可参与光盘打卡行动',
          url: 'https://oms.cestech.com.cn/BkpKnowledge/img/dd918d371e1249f78dba49a2579ea38f/光盘行动.png',
          btn: '立即打卡',
          id: '1656523212312432104',
        },
        {
          title: '厨余分类',
          tip: '如剩菜、剩饭、果皮、蛋壳、茶渣、骨头等均属于厨余垃圾',
          url: 'https://oms.cestech.com.cn/BkpKnowledge/img/2b8a82317c404a098096bf81e8d9e5e6/厨余.png',
          btn: '立即分类',
          id: '1656123123213432104',
        },
        {
          title: '分类随手拍',
          tip: '对自身分类行为或他人乱扔垃圾等垃圾分类相关行为进行随手拍，共同维护环境卫生',
          url: 'https://oms.cestech.com.cn/BkpKnowledge/img/6f59bfeaa6f0470dac4efa8b6faee9b7/随手拍.png',
          btn: '立即参与',
          id: '1656123123213432231',
        },
        {
          title: '建言献策',
          tip: '对垃圾分类相关工作开展事宜，发表建议意见',
          url: 'https://oms.cestech.com.cn/BkpKnowledge/img/49bfb01ac7b54929bfbc7dc3875e69f5/投诉建议.png',
          btn: '立即请往',
          id: '1656123123213432222',
        },
        {
          title: '分类活动/培训参与',
          tip: '参与线下/线上垃圾分类相关活动或培训等事宜',
          url: 'https://oms.cestech.com.cn/BkpKnowledge/img/5c26eeed42f44c5aa7f72ad5a0bc7e73/活动.png',
          btn: '立即参与',
        },
      ],
      text1: [],
      loginBtn: true,
    }
  },
  onShow() {
    this.loginBtn = uni.getStorageSync('phone') ? false : true
  },
  methods: {
    go_clock(item) {
      if (item.title === '分类活动/培训参与') {
        uni.redirectTo({
          url: `/pageB/activitycustomized/index`,
        })
      } else if (item.title === '21天打卡') {
        uni.redirectTo({
          url: `/pageD/signIn/signIn`,
        })
      } else {
        uni.redirectTo({
          url: `/pageD/check/check?title=${item.title}&id=${item.id}`,
        })
      }
    },
    getPhoneNumber(e, item) {
      if (e.detail.errMsg != 'getPhoneNumber:ok') {
        return
      }

      let code = e.detail.code
      uni.login({
        provider: 'weixin',
        success: res => {
          let myPhone = ''
          if (uni.getStorageSync('scene') != 'undefined') {
            myPhone = uni.getStorageSync('scene')
          }
          uni.showLoading({
            title: '加载中',
          })
          uni.request({
            url: `https://wj.api.cestech.com.cn:48002/mission_b/wx/WxApi/miniProgramLogin?code=${res.code}`,
            // header: {
            // 	'x-Resource-Token':uni.getStorageSync('xtoken'),
            // 	'X-Resource-Token': '' //自定义请求头信息
            // },
            method: 'GET',
            success: res => {
              uni.setStorageSync(
                'session_key',
                res.data.result.token.session_key
              )
              uni.setStorageSync('openid', res.data.result.token.openid)
              uni.setStorageSync('users', res.data.result.user)
              this.users = res.data.result.user
              this.txt = res.data.result.user.role.split(',')

              uni.setStorageSync('nikename', res.data.result.user.nickName)
              this.name = uni.getStorageSync('nikename')
              uni.setStorageSync('avatarUrl', res.data.result.user.avatarUrl)
              this.avatarUrl = uni.getStorageSync('avatarUrl')
              uni.request({
                url: `https://wj.api.cestech.com.cn:48002/mission_b/wx/WxApi/getPhoneNumber?code=${code}&openid=${res.data.result.token.openid}`,

                method: 'GET',
                success: res => {
                  uni.setStorageSync('xtoken', res.data.result.access_tokenn)

                  this.iphone = res.data.result.phone_info.phoneNumber
                  let data = {}
                  data = this.users
                  data.phone = this.iphone

                  uni.request({
                    url: 'https://wj.api.cestech.com.cn:48002/mission_b/wx/WxApi/edit',
                    header: {
                      'x-Resource-Token': uni.getStorageSync('xtoken'),
                    },
                    data: data,
                    method: 'POST',
                    success: res => {},
                  })
                  uni.hideLoading()
                  uni.setStorageSync('iphone', this.iphone)
                  uni.setStorageSync('phone', this.iphone)
                  // this.userinfo()
                  this.show_head = false
                  this.showmy = true

                  uni.showToast({
                    title: '登录成功',
                    icon: 'success',
                  })
                  setTimeout(() => {
                    this.go_clock(item)
                  }, 1000)
                },
              })
            },
          })
        },
      })
    },
  },
}
</script>

<style lang="scss" scoped>
page {
  background: #fff;
}

.min {
  position: relative;

  .headers {
    z-index: -1;
    width: 750rpx;
    height: 140rpx;
    display: flex;
    /* background-image: url('@/static/assets/head-bg.png'); */

    .head-title {
      color: #fff;
      font-weight: 550;
      font-size: 36rpx;
      margin: 0 auto;
      margin-top: 25rpx;
    }
  }

  .main {
    // z-index: ;
    min-height: 1000rpx;
    background: #fff;
    border-radius: 33rpx 33rpx 0rpx 0rpx;
    position: relative;
    bottom: 40rpx;

    .conetent-body {
      overflow: hidden;
      width: 100%;

      padding-top: 30rpx;

      .u-swiper__indicator {
        bottom: -10px !important;
      }

      .baner {
        margin: 20rpx 0;
        width: 100%;
        height: 220rpx;
      }

      .index-list {
        .item {
          border-radius: 35rpx;
          padding: 20rpx 30rpx;
          width: 700rpx;
          margin: 0 auto;
          border: 2rpx solid rgb(100, 200, 155);
          display: flex;
          align-items: center;
          justify-content: space-between;
          margin-bottom: 30rpx;

          .btn {
            background: rgb(41, 206, 140);
            color: #fff;
            padding: 10rpx 20rpx;
            border-radius: 30rpx;
            font-size: 30rpx;
          }

          .item-left {
            display: flex;
            align-items: center;

            .title {
              font-weight: 600;
              font-size: 30rpx;
            }

            .details {
              color: #999;
              margin-top: 5rpx;
              width: 320rpx;
              display: -webkit-box;
              -webkit-box-orient: vertical;
              -webkit-line-clamp: 2;
              overflow: hidden;
              // overflow: hidden;
              // text-overflow:ellipsis;
              // white-space: nowrap;
            }
          }
        }
      }

      .noticebar-name {
        width: 700rpx;
        margin: 30rpx auto;
        display: flex;
        align-items: center;
        background: rgb(253, 246, 236);

        // margin-right: 15rpx;
        .u-notice-bar {
          padding: 18rpx 10rpx !important;
          // background: #fff !important
        }

        .not-title {
          padding: 0 0 0 20rpx;
          font-weight: 550;
        }
      }

      .indicators {
        @include flex(row);
        margin-top: 10px;
        justify-content: center;

        &__dot {
          height: 5px;
          width: 15px;
          border-radius: 5px;
          background-color: rgba(29, 209, 155, 0.3);
          margin: 0 3px;
          transition: background-color 0.3s;

          &--active {
            background-color: rgba(29, 209, 155, 1);
          }
        }
      }
    }

    .u-flex {
      display: flex;
      flex-direction: row;
      align-items: center;
    }
  }
}
</style>
