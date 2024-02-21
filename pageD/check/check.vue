<template>
  <view class="content">
    <!-- 上传图片 -->
    <view class="upload" v-if="title != '建言献策'">
      <view class="title">
        拍照打卡:
        <view
          v-if="title == '光盘行动' || checkInfo.status == 1"
          style="font-size: 12px; color: #a2aca8">
          每日{{ timeListText }}可打卡
        </view>
      </view>
      <u-upload
        :fileList="fileList1"
        @afterRead="afterRead"
        @delete="deletePic"
        name="1"
        :multiple="true"
        :maxCount="2"
        width="200rpx"
        height="200rpx"
        :deletable="true"
        :previewFullImage="true"
        uploadText="选择图片">
        <!-- 这张图片就是自定义的图片，地址填写自己本地的就行 -->
        <!-- <image src="/static/function/uploadImg.png" mode="widthFix" style="width: 112rpx;height: 110rpx;"></image> -->
      </u-upload>
    </view>
    <view class="text">
      <view class="title"> 分享心得： </view>
      <u--textarea
        v-model="punchCardDescribe"
        placeholder="请输入内容"></u--textarea>
    </view>

    <view class="text" v-if="isShowMap">
      <view class="title"> 我的位置： </view>
      <view class="address title">{{ address }}</view>
      <view>
        <!-- <map style="width: 100%; height: 300px;" :longitude="104.056919" :latitude="30.603235" :markers="markers">
				</map> -->
        <map
          :longitude="longitude"
          :latitude="latitude"
          style="width: 700rpx; height: 400rpx"
          :show-location="true">
        </map>
      </view>
    </view>
    <view class="btn" @click="toCheckSuccess"> 确认 </view>
  </view>
</template>

<script>
import QQMapWX from '@/util/qqmap-wx-jssdk.min.js'
const qqmapsdk = new QQMapWX({
  key: 'N3SBZ-PBXC7-A67XP-HX4UN-ZHVLE-MIFRN', // 必填
})
export default {
  data() {
    return {
      punchCardDescribe: '',
      longitude: null,
      latitude: null,
      address: '',
      // 上传图片
      fileList1: [],
      timeListText: '',
      title: '',
      titleId: '',
      isShowMap: false,
    }
  },
  onLoad(options) {
    if (options.title) {
      this.title = options.title
      this.titleId = options.id
    }
    this.getPosition()

    if (this.titleId) {
      uni.request({
        url: `https://wj.api.cestech.com.cn:48002/mission_b/xcx_api/bkbPunchCardRecord/requestTime`,
        data: {
          id: this.titleId,
        },
        method: 'get',
        success: res => {
          console.log(res)
          res.data.result.map(item => {
            this.timeListText += `${item.startTime}-${item.endTime}, `
          })
          this.timeListText = this.timeListText.slice(
            0,
            this.timeListText.length - 2
          )
        },
      })
      uni.request({
        url: `https://wj.api.cestech.com.cn:48002/mission_b/mission/bkbPunchCardSetting/queryById`,
        data: {
          id: this.titleId,
        },
        method: 'GET',
        success: res => {
          console.log('asda', res)
          if (res.data.result.area == '是') this.isShowMap = true
          else this.isShowMap = false
        },
      })
    }
  },
  methods: {
    getPosition() {
      uni.getLocation({
        type: 'gcj02',
        isHighAccuracy: true,
        success: lb => {
          this.latitude = lb.latitude
          this.longitude = lb.longitude
          qqmapsdk.reverseGeocoder({
            //Object格式
            location: {
              latitude: this.latitude,
              longitude: this.longitude,
            },
            success: res => {
              //成功后的回调
              this.address = res.result.address
            },
            fail: error => {
              console.error(error)
            },
            complete: res => {},
          })
        },
        fail: res => {
          console.log(res)
        },
      })
    },
    punchCardRecord(imgurl) {
      const phone = uni.getStorageSync('phone')
      const headSculpture = uni.getStorageSync('avatarUrl')
      const nickName = uni.getStorageSync('nikename')
      uni.request({
        url: `https://wj.api.cestech.com.cn:48002/mission_b/xcx_api/bkbPunchCardRecord/add?punchCardPhone=${phone}&type=${this.title}&punchType=${this.title}&punchCardAddress=${this.address}&headSculpture=${headSculpture}&nickName=${nickName}`,
        data: {
          punchCardPhone: uni.getStorageSync('phone'),
          type: this.title,
          punchCardDescribe: this.punchCardDescribe,
          punchCardArrayUrl: imgurl,
          punchType: this.title,
          punchCardAddress: this.address,
          headSculpture,
          nickName,
        },
        method: 'POST',
        success: res => {
          if (res.data.code == 200)
            uni.redirectTo({
              url: `/pageD/checkSuccess/checkSuccess?title=${this.title}`,
            })
          else
            uni.showToast({
              title: res.data.message,
              icon: 'none',
            })
        },
      })
    },
    //删除图片
    deletePic(e) {
      console.log(e)
      this[`fileList${e.name}`].splice(e.index, 1)
    },
    // 新增图片
    async afterRead(event) {
      console.log(event)
      // 当设置 mutiple 为 true 时, file 为数组格式，否则为对象格式
      let lists = [].concat(event.file)
      let fileListLen = this[`fileList${event.name}`].length
      lists.map(item => {
        this[`fileList${event.name}`].push({
          ...item,
          status: 'uploading',
          message: '上传中',
        })
      })
      for (let i = 0; i < lists.length; i++) {
        const result = await this.uploadFilePromise(lists[i].url)
        let item = this[`fileList${event.name}`][fileListLen]
        this[`fileList${event.name}`].splice(
          fileListLen,
          1,
          Object.assign(item, {
            status: 'success',
            message: '',
            url: result,
          })
        )
        fileListLen++
      }
    },
    //上传图片
    uploadFilePromise(url) {
      return new Promise((resolve, reject) => {
        let a = uni.uploadFile({
          //url: this.$common.domain+'/api/common/upload', // 仅为示例，非真实的接口地址
          url: 'https://wj.api.cestech.com.cn:48002/mission_b/xcx_api/file/imgUpload', // 仅为示例，非真实的接口地址
          filePath: url,
          name: 'file',
          formData: {
            user: 'test',
          },
          success: res => {
            resolve(JSON.parse(res.data).result)
          },
        })
      })
    },

    toCheckSuccess() {
      // let imgurl = ''
      // this.fileList1.forEach((item, index) => {
      //   imgurl = imgurl + item.url + ','
      //   // imgurl = item.url
      // })
      // imgurl = imgurl.slice(0, -1)

      // if (!imgurl) {
      //   uni.showToast({
      //     title: '请上传图片后再打卡',
      //     icon: 'none',
      //   })
      //   return
      // }
      let imgurl = ''
      this.fileList1.forEach((item, index) => {
        imgurl = imgurl + item.url + ','
      })

      imgurl = imgurl.slice(0, -1)
      if (!imgurl && this.title != '建言献策') {
        uni.showToast({
          icon: 'none',
          title: '请拍照后, 再打卡',
        })
        return
      }
      if (this.title == '建言献策' && !this.punchCardDescribe) {
        uni.showToast({
          icon: 'none',
          title: '请填写分享心得后, 再打卡',
        })
        return
      }
      if (this.isShowMap)
        uni.request({
          url: `https://wj.api.cestech.com.cn:48002/mission_b/xcx_api/bkbPunchCardRecord/shiyangMap`,
          data: {
            x: this.longitude,
            y: this.latitude,
          },
          method: 'GET',
          success: res => {
            if (res.data === true) {
              this.punchCardRecord(imgurl)
            } else {
              uni.showToast({
                title: '不在打卡范围内',
                icon: 'none',
              })
              return
            }
          },
        })
      else this.punchCardRecord(imgurl)
    },
  },
}
</script>

<style lang="scss">
.btn {
  width: 400rpx;
  height: 100rpx;
  background: #29ce8c;
  border-radius: 20rpx;
  line-height: 100rpx;
  color: #fff;
  text-align: center;
  margin: 100rpx auto;
}

.title {
  margin-bottom: 20rpx;
  font-size: 28rpx;
}

.content {
  padding: 30rpx;
  padding-top: 0;
}

.upload {
  margin-top: 40rpx;
}
.text {
  margin-top: 40rpx;
}
</style>
