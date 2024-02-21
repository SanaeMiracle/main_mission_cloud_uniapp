<template>
  <view class="content">
    <view class="u-p">
      <bindinfoform @success="submit" ref="bindinfoform" :details="details">
      </bindinfoform>
    </view>
  </view>
</template>

<script>
import authorize from '@/common/auth.js'
import { bindinfo } from '@/common/api.js'
import { mapState } from 'vuex'
import bindinfoform from './components/bindinfo.vue'

export default {
  components: {
    bindinfoform,
  },
  data() {
    return {
      globalData: getApp().globalData,
      btnLoading: false,
      details: {},
      task_id: null,
    }
  },
  computed: {
    ...mapState({
      user: state => state.user,
      cityDataTree: state => state.cityDataTree,
    }),
    current() {
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
  onLoad(option) {
    if (option.current) {
      this.current = option.current
    }
    if (option.task_id) {
      this.task_id = option.task_id
    }

    // uni.request({
    //   url: `https://wj.api.cestech.com.cn:48002/mission_b/v1/user/info?id=${userid}`,

    //   method: "GET",
    //   success: (res) => {
    //     console.log(res.data.result, 111);
    //     // this.$store.commit('refreshUser',
    //     //   data)
    //     this.details = res.data.result

    //   }

    // })
  },

  methods: {
    submit(params) {
      if (this.task_id) {
        params.task_id = this.task_id
      }
      this.$refs.bindinfoform.btnLoading = true
      params.id = uni.getStorageSync('users').id
      uni.request({
        url: `https://wj.api.cestech.com.cn:48002/mission_b/wx/WxApi/edit`,
        header: {
          'x-Resource-Token': uni.getStorageSync('xtoken'),
          //'x-Resource-Token':uni.getStorageSync('xtoken'), //自定义请求头信息
        },
        data: params,
        method: 'POST',
        success: res => {
          console.log(res.data.result)
          // this.$store.commit('refreshUser',
          //   data)

          this.goBack()
          uni.showToast({
            title: '修改成功',
            icon: 'success',
          })
          this.$refs.bindinfoform.btnLoading = false
        },
      })

      // bindinfo(params).then(({
      //   data,
      //   msg
      // }) => {
      //   this.$store.commit('refreshUser',
      //     data)

      //   this.goBack()
      //   uni.showToast({
      //     title: msg,
      //     icon: 'success',
      //   });
      // }).finally(() => {
      //   this.$refs.bindinfoform.btnLoading = false;
      // })
    },
    goBack() {
      let pages = getCurrentPages() // 当前页面
      let beforePage = pages[pages.length - 2]
      setTimeout(() => {
        uni.navigateBack({
          delta: 1,
          success: function () {
            beforePage.$vm.init() // 执行前一个页面的init 来实现刷新
          },
        })
      }, 300)
    },
    getUserProfile(e) {
      authorize.getUserProfile().then(res => {
        uni.showToast({
          title: res.msg,
          icon: 'success',
        })
      })
    },
    getPhoneNumber(e) {
      authorize.bindPhone(e.detail).then(res => {
        uni.showToast({
          title: res.msg,
          icon: 'success',
        })
      })
    },
  },
}
</script>
<style>
page {
  width: 100%;
}
</style>
<style lang="scss" scoped>
/deep/ .u-form-item__body__left {
  min-width: 5em;
}
</style>
