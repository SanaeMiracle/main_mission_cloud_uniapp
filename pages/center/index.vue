<!-- 个人中心 -->
<template>
  <view class="components-theme">
    <view class="u-nav">
      <navbar
        :safeAreaInsetTop="true"
        :allTitle="false"
        height="55"
        placeholder
        imageSrc="http://cdn.cestech.com.cn/BkpKnowledge/img/9bfc5177f12447f5b2c5f3c618679068.jpg">
        <view class="u-nav-slot" slot="left">
          <u--text text="垃圾分类宣教云" bold></u--text>
        </view>
      </navbar>
    </view>
    <!-- 顶部 -->
    <view class="UCenter-bg">
      <block>
        <view class="top-box header-images u-flex u-flex-row u-row-between">
          <view class="left" v-if="showmy">
            <view class="left-name">
              <u--text
                :text="name"
                bold
                size="17"
                style="width: 200rpx"></u--text>
            </view>

            <view class="left-info">
              <u--text :text="iphone" type="info"></u--text>
            </view>
          </view>
          <!-- <view class="left-button" v-if="show_head">
						<button type="primary" shape="circle" open-type="chooseAvatar" @chooseavatar="bindchooseavatar">
							请登录
						</button>
					</view>
					<view class="left-button name-input" v-if="show_name">
						<input type="nickname" placeholder="请输入昵称" v-model="name" @input="bindinput" @blur="onNickName"
							:disabled="showname" />
					</view>
					<view class="left-button" v-if="show_phone">
						<u-button type="primary" shape="circle" text="授权手机号" @getphonenumber="getPhoneNumber"
							open-type="getPhoneNumber">
						</u-button>
					</view> -->
          <view class="left-button" v-if="show_head">
            <u-button
              type="primary"
              shape="circle"
              text="请登录"
              @getphonenumber="getPhoneNumber"
              open-type="getPhoneNumber">
            </u-button>
          </view>
          <view v-if="showmy">
            <u-avatar :src="avatarUrl" size="100"></u-avatar>
          </view>
        </view>
      </block>
    </view>
    <view class="u-m-t">
      <u-cell-group :border="false">
        <u-cell
          title="个人信息"
          @click="goAuth"
          size="large"
          :isLink="true"
          :border="false"
          :rightIconStyle="{
            color: '#333333',
          }">
          <u--image
            slot="icon"
            mode="heightFix"
            src="/static/assets/center/1.png"
            width="16px"
            height="16px">
          </u--image>
        </u-cell>
        <!-- <u-cell title="我的积分" @click="integral" size="large" :isLink="true" :border="false" :rightIconStyle="{
				    color:'#333333'
				}">
          <u--image slot="icon" mode="heightFix" src="/static/assets/center/5.png" width="16px" height="16px">
          </u--image>
          <text slot="value" class="u-slot-value">{{users.points}}</text>
        </u-cell>
        <u-cell title="积分排行" @click="ranking" size="large" :isLink="true" :border="false" :rightIconStyle="{
				    color:'#333333'
				}">
          <u--image slot="icon" mode="heightFix" src="/static/assets/center/6.png" width="16px" height="16px">
          </u--image>
        </u-cell> -->
        <!-- <u-cell title="督导员工具" v-if="users.role===1" @click="goSupervisor" size="large" :isLink="true" :border="false"
          :rightIconStyle="{
                    color:'#333333'
                }">
          <u--image slot="icon" mode="heightFix" src="/static/assets/center/2.png" width="16px" height="16px">
          </u--image>
        </u-cell> -->
        <u-cell
          title="打卡记录"
          @click="checkRecoed"
          size="large"
          :isLink="true"
          :border="false"
          :rightIconStyle="{
            color: '#333333',
          }">
          <u--image
            slot="icon"
            mode="heightFix"
            src="/static/assets/center/6.png"
            width="16px"
            height="16px">
          </u--image>
        </u-cell>
        <u-cell
          title="督导员工具"
          v-if="
            txt.filter(function (e) {
              return e == '1'
            }).length != 0
          "
          @click="goSupervisor"
          size="large"
          :isLink="true"
          :border="false"
          :rightIconStyle="{
            color: '#333333',
          }">
          <u--image
            slot="icon"
            mode="heightFix"
            src="/static/assets/center/2.png"
            width="16px"
            height="16px">
          </u--image>
        </u-cell>

        <u-cell
          title="活动任务"
          v-if="
            txt.filter(function (e) {
              return e == '2'
            }).length != 0
          "
          @click="goActivity"
          size="large"
          :isLink="true"
          :border="false"
          :rightIconStyle="{
            color: '#333333',
          }">
          <u--image
            slot="icon"
            mode="heightFix"
            src="/static/assets/center/2.png"
            width="16px"
            height="16px">
          </u--image>
        </u-cell>
        <u-cell
          title="我的消息"
          @click="goMessage"
          size="large"
          :isLink="true"
          :border="false"
          :rightIconStyle="{
            color: '#333333',
          }">
          <u--image
            slot="icon"
            mode="heightFix"
            src="/static/assets/center/4.png"
            width="20px"
            height="20px">
          </u--image>

          <u-badge slot="value" type="error" :value="message"> </u-badge>
        </u-cell>
        <u-cell
          title="问卷管理"
          v-if="
            txt.filter(function (e) {
              return e == '3'
            }).length != 0
          "
          @click="questionnaire"
          size="large"
          :isLink="true"
          :border="false"
          :rightIconStyle="{
            color: '#333333',
          }">
          <u--image
            slot="icon"
            mode="heightFix"
            src="/static/assets/center/7.png"
            width="22px"
            height="22px">
          </u--image>
        </u-cell>
        <u-cell
          title="预约订单"
          @click="order"
          size="large"
          :isLink="true"
          :border="false"
          :rightIconStyle="{
            color: '#333333',
          }">
          <u--image
            slot="icon"
            mode="heightFix"
            src="/static/assets/center/8.png"
            width="18px"
            height="18px">
          </u--image>
        </u-cell>
        <u-cell
          title="我的培训任务"
          v-if="
            txt.filter(function (e) {
              return e == '4'
            }).length != 0
          "
          @click="my_task"
          size="large"
          :isLink="true"
          :border="false"
          :rightIconStyle="{
            color: '#333333',
          }">
          <u--image
            slot="icon"
            mode="heightFix"
            src="/static/assets/center/9.png"
            width="18px"
            height="18px">
          </u--image>
        </u-cell>
        <u-cell
          title="收货地址"
          @click="address"
          size="large"
          :isLink="true"
          :border="false"
          :rightIconStyle="{
            color: '#333333',
          }">
          <u--image
            slot="icon"
            mode="heightFix"
            src="/static/assets/center/address.png"
            width="18px"
            height="18px">
          </u--image>
        </u-cell>
      </u-cell-group>
    </view>
  </view>
</template>

<script>
import authorize from '@/common/auth.js'
import { mapState } from 'vuex'
import { message as getMessage } from '@/common/api.js'
export default {
  data() {
    return {
      // pic: getApp().globalData.user,
      topBackGroupImageIndex: 5,
      redirect: '',
      message: '0',
      points: 0, //积分
      avatarUrl: '',
      name: '',
      showmy: false,
      show_head: true,
      show_name: false,
      show_phone: false,
      showname: false,
      phone_s: false,
      iphone: '',
      users: {},
      txt: [],
    }
  },
  onTabItemTap() {
    this.userinfo()
    // authorize.login()
  },
  onLoad(options) {
    this.infoInit()
  },
  computed: {
    ...mapState({
      user: state => state.user,
    }),
    nick_name() {
      if (this.user.name) {
        return this.user.name
      }
      if (this.user.nick_name) {
        return this.user.nick_name
      }
      return '微信用户'
    },
    message() {
      if (this.user.message) {
        return this.user.message > 99 ? '99+' : this.user.message
      }
      return '0'
    },
    // phone() {
    // 	this.phone_s=true
    // 	if (this.user.phone) {

    // 		return this.user.phone
    // 	}

    // 	return ''
    // },
    loginStatus() {
      if (!this.$store.state.user.nick_name) {
        return 0
      } else if (!this.$store.state.user.phone) {
        return 1
      } else if (!this.$store.state.user.reg_status) {
        return 2
      } else {
        return 3
      }
      return 0
    },
  },
  // 分享小程序
  onShareAppMessage(res) {
    return {
      title: '赶紧分享给身边的亲友吧',
    }
  },
  watch: {
    topBackGroupImageIndex(val) {
      if (val == 4 || val == 5) {
        this.spaceShow = true
      } else {
        this.spaceShow = false
      }
    },
  },
  onShow() {
    this.users = uni.getStorageSync('users')
    uni.request({
      url: 'https://wj.api.cestech.com.cn:48002/mission_b/v1/user/message',
      header: {
        'x-Resource-Token': uni.getStorageSync('xtoken'),
      },
      // data: {
      // 	id: uni.getStorageSync('users').id
      // },
      method: 'GET',
      success: res => {
        console.log(res)
        this.message =
          res.data.result > 99 ? '99+' : res.data > 0 ? res.data : 0
      },
    })
    this.infoInit()
    // getMessage().then((res) => {
    //   this.message = res.data > 99 ? '99+' : (res.data > 0 ? res.data : 0);
    // })
  },
  mounted() {},
  methods: {
    infoInit() {
      if (uni.getStorageSync('iphone')) {
        this.iphone = uni.getStorageSync('iphone')
      }
      if (uni.getStorageSync('avatarUrl')) {
        this.avatarUrl = uni.getStorageSync('avatarUrl')
        this.show_head = false
        this.show_name = false
        this.show_phone = false
        this.showmy = true
      } else {
      }
      if (uni.getStorageSync('nikename')) {
        this.name = uni.getStorageSync('nikename')
        this.show_head = false
        this.show_name = false
        this.show_phone = false
        this.showmy = true
      } else {
        this.show_head = true
        this.phone_s = true
      }
      this.userinfo()
    },
    init() {
      this.name = ''
      this.avatarUrl = ''
      this.userinfo()
      console.log('xxx被调')
    },
    address() {
      uni.navigateTo({
        url: '/pageD/address/index',
      })
    },
    my_task() {
      uni.navigateTo({
        url: '/pageB/activity_manager/index',
      })
    },
    questionnaire() {
      uni.navigateTo({
        url: '/pageB/questionnaire/index',
      })
    },
    order() {
      uni.navigateTo({
        url: '/pageB/appointmentorder/index',
      })
    },
    empower() {
      uni.navigateTo({
        url: '/pageC/auth/login',
      })
    },
    checkRecoed() {
      uni.navigateTo({
        url: '/pageD/check/checkRecord/checkRecord',
      })
    },

    //获取用户头像
    bindchooseavatar(e) {
      console.log(e)
      if (e.detail.avatarUrl) {
        this.show_head = false
        this.show_name = true
        this.show_phone = false
        // this.uploadFilePromise(e.detail.avatarUrl)
        uni.setStorageSync('avatarUrl', e.detail.avatarUrl)
        this.avatarUrl = uni.getStorageSync('avatarUrl')
        console.log(uni.getStorageSync('avatarUrl'), 'avatarUrl')
        // authorize.login()
      }
    },
    uploadFilePromise(avatarUrl) {
      //调后台接口上传图片  这里的avatarUrl拿到的是微信地址
      uni.request({
        url: 'https://example.weixin.qq.com/upload',

        data: avatarUrl,
        success(res) {
          // console.log(res.data.avatarUrl);
          // const data = res.data
          //do something
          completeMemberInfo(res.data.avatarUrl)
        },
      })
    },
    completeMemberInfo(data) {
      console.log(data, '头像地址')
      //接收 avatarUrl / nickName 调接口完善头像或者昵称信息
    },

    bindinput(e) {
      console.log(e.detail.value, '昵称')
      uni.setStorageSync('nikename', e.detail.value)
      this.name = uni.getStorageSync('nikename')
      // authorize.login()
    },
    onNickName(e) {
      // console.log(e.detail.value, '昵称')
      if (e.detail.value) {
        this.show_head = false
        this.show_name = false
        this.show_phone = true
        uni.setStorageSync('nikename', e.detail.value)
        this.name = uni.getStorageSync('nikename')
      }
      // this.completeMemberInfo(e.detail.value)
    },
    //获取用户个人信息
    userinfo() {
      uni.request({
        url: 'https://wj.api.cestech.com.cn:48002/mission_b/v1/user/info',
        header: {
          'x-Resource-Token': uni.getStorageSync('xtoken'),
        },
        // data: {
        // 	id: uni.getStorageSync('users').id
        // },
        method: 'GET',
        success: res => {
          console.log(res, '用户')

          this.points = res.data.result.records[0].points
          this.users = res.data.result.records[0]
          uni.setStorageSync('users', res.data.result.records[0])
          this.txt = res.data.result.records[0].role.split(',')
          this.name = res.data.result.records[0].nickName
          this.avatarUrl = res.data.result.records[0].avatarUrl
        },
      })
    },
    // 个人信息
    goAuth() {
      uni.navigateTo({
        url: '/pageC/auth/register',
      })
    },
    //我的积分
    integral() {
      uni.navigateTo({
        url: '/pageA/integral/index',
      })
    },
    //积分排名
    ranking() {
      uni.navigateTo({
        url: '/pageA/ranking/index',
      })
    },
    goSupervisor() {
      uni.navigateTo({
        url: '/pages/supervisor/task',
      })
      return
      uni.navigateTo({
        url: '/pages/supervisor/index',
      })
    },
    goActivity() {
      uni.navigateTo({
        url: '/pages/activity_manager/index',
      })
    },
    goLogin() {
      uni.navigateTo({
        url: '/pageC/auth/login',
      })
    },
    goMessage() {
      uni.navigateTo({
        url: '/pageC/message/list',
      })
    },

    //拨打固定电话
    callPhoneNumber() {
      uni.makePhoneCall({
        phoneNumber: '18629591093',
      })
    },

    getUserProfile() {
      authorize.getUserProfile().then(res => {
        // uni.showToast({
        // 	title: res.msg,
        // 	icon: 'success',
        // });
      })
    },
    getPhoneNumber(e) {
      if (e.detail.errMsg != 'getPhoneNumber:ok') {
        return
      }
      // authorize.login()
      // this.show_phone = false
      // this.showmy = true

      console.log(this.phone, '手机号')
      console.log('qq', e.detail)
      let code = e.detail.code
      uni.login({
        provider: 'weixin',
        success: res => {
          console.log(res, 'xxxx')
          let myPhone = ''
          if (uni.getStorageSync('scene') != 'undefined') {
            myPhone = uni.getStorageSync('scene')
          }
          uni.showLoading({
            title: '加载中',
          })
          uni.request({
            // http: //192.168.100.56:8087/mission_b/wx/WxApi/miniProgramLogin?code=111&phone=111
            url: `https://wj.api.cestech.com.cn:48002/mission_b/wx/WxApi/miniProgramLogin?code=${res.code}`,
            // header: {
            // 	'x-Resource-Token':uni.getStorageSync('xtoken'),
            // 	'X-Resource-Token': '' //自定义请求头信息
            // },
            method: 'GET',
            success: res => {
              console.log(res.data.result.token.session_key, '秘钥')
              uni.setStorageSync(
                'session_key',
                res.data.result.token.session_key
              )
              uni.setStorageSync('openid', res.data.result.token.openid)
              console.log(res.data.result.token, '用户信息')
              uni.setStorageSync('users', res.data.result.user)
              this.users = res.data.result.user
              this.txt = res.data.result.user.role.split(',')
              console.log(this.txt, '身份认证')
              console.log(res.data.result.user.nickName, 'ccccxxxx')

              uni.setStorageSync('nikename', res.data.result.user.nickName)
              this.name = uni.getStorageSync('nikename')
              uni.setStorageSync('avatarUrl', res.data.result.user.avatarUrl)
              this.avatarUrl = uni.getStorageSync('avatarUrl')
              uni.request({
                url: `https://wj.api.cestech.com.cn:48002/mission_b/wx/WxApi/getPhoneNumber?code=${code}&openid=${res.data.result.token.openid}`,

                method: 'GET',
                success: res => {
                  uni.setStorageSync('xtoken', res.data.result.access_tokenn)
                  console.log(res.data.result.access_tokenn, '解析完成')

                  console.log(res.data.result.access_tokenn, '数据')
                  // this.checkPhone(res.data.result.phoneNumber.phoneNumber)
                  // this.iphone = res.data.result.phoneNumber
                  this.iphone = res.data.result.phone_info.phoneNumber
                  console.log(this.iphone, 'this.iphone')
                  let data = {}
                  data = this.users
                  // data.nickName = this.name
                  // data.avatarUrl = this.avatarUrl
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
                  this.userinfo()
                  // authorize.login()
                  this.show_head = false
                  this.showmy = true

                  uni.showToast({
                    title: '登录成功',
                    icon: 'success',
                  })
                },
              })
              //获取用户信息
              // uni.request({
              // 	url: `https://wj.api.cestech.com.cn:48002/mission_b/wx/WxApi/list?openid=${res.data.result.token.openid}`,
              // 	header: {
              // 		'x-Resource-Token': uni.getStorageSync('xtoken'),

              // 	},
              // 	method: "GET",
              // 	success: (res) => {
              // 		console.log(res, '用户')

              // 	}

              // })
            },
          })

          //这里获取的是用户的code，用来换取 openid、unionid、session_key 等信息，再将信息丢给后台自己的登录业务就行了
        },
      })
    },
  },
}
</script>
<style>
page {
  background-color: #f7f7f7;
}
</style>

<style lang="scss" scoped>
.UCenter-bg {
  background: #fff;
  background-size: 100% 100%;
  /* background-size: cover; */
  display: flex;
  justify-content: center;
  overflow: hidden;
  position: relative;
  flex-direction: column;
  align-items: center;
  font-weight: 300;
  height: 160px;

  .top-box {
    width: 100%;
    padding: 30rpx;
    height: 100%;

    .left {
      padding-left: 30rpx;
      height: 100%;
      flex: 1;
      display: flex;
      justify-content: center;
      flex-direction: column;

      .left-name {
        margin-bottom: 15rpx;
        display: flex;
        align-items: center;

        .iocn {
          // position: relative;
          // right: 120px;
        }
      }
    }

    .left-button {
      /deep/ .u-button__text {
        padding: 0 10px;
      }
    }

    .name-input {
      /deep/ .data-v-0f3320de {
        width: 260rpx;
        padding-left: 40rpx;
        height: 80rpx;
        background: #3bcf99;
        border-radius: 200rpx;
        color: #ffffff !important;

        .input::-webkit-input-placeholder {
          /* placeholder颜色 */

          /* placeholder字体大小 */
          font-size: 12px;
        }
      }
    }
  }
}

// /deep/ .input:placeholder-shown {
// 	font-size: 20rpx;
// 	color: #fff;
// }
</style>
