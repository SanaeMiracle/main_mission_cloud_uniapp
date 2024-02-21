<template>
  <view class="content">
    <u-loading-page :loading="pageLoading"></u-loading-page>
    <u--form
      v-if="!pageLoading"
      labelPosition="left"
      :model="form"
      ref="form"
      labelWidth="5em">
      <u-form-item
        label="昵称"
        prop="nickName"
        borderBottom
        ref="nickName"
        required>
        <u--input
          v-model="form.nickName"
          border="none"
          placeholder="请填写昵称"
          clearable></u--input>
      </u-form-item>
      <u-form-item label="头像" borderBottom>
        <u-avatar
          :src="form.avatarUrl"
          v-if="form.avatarUrl"
          @click="avaUrl"></u-avatar>
      </u-form-item>
      <u-form-item label="姓名" prop="name" borderBottom ref="name" required>
        <u--input
          v-model="form.name"
          border="none"
          placeholder="姓名,只能为中文"
          clearable></u--input>
      </u-form-item>

      <u-form-item
        label="性别"
        prop="gender"
        borderBottom
        @click="
          showgender = true
          hideKeyboard()
        "
        required>
        <u--input
          v-model="form.gender_text"
          disabled
          disabledColor="#ffffff"
          placeholder="请选择性别"
          border="none">
        </u--input>
        <u-icon slot="right" name="arrow-right"></u-icon>
      </u-form-item>
      <u-form-item
        label="所在地区"
        prop="region"
        borderBottom
        ref="region"
        required
        v-if="city">
        <selfPicker
          level="3"
          :defaultIds="regionIdx"
          @change="regionChange"
          idKey="value"
          name="name"
          children="children"
          :columnData="cityDataTree"></selfPicker>
        <u-icon slot="right" name="arrow-right"></u-icon>
      </u-form-item>
      <u-form-item
        label="所在地区"
        prop="region"
        borderBottom
        ref="region"
        required
        v-if="citys"
        @click="city_s">
        <u--input
          v-model="form.dq"
          disabled
          disabledColor="#ffffff"
          placeholder="请选择小区"
          border="none">
        </u--input>
        <u-icon slot="right" name="arrow-right"></u-icon>
      </u-form-item>
      <u-form-item
        @click="xq"
        label="所在小区"
        prop="community"
        borderBottom
        ref="community"
        v-if="communitydictTexts">
        <u--input
          v-model="form.xq"
          disabled
          disabledColor="#ffffff"
          placeholder="请选择小区"
          border="none">
        </u--input>
        <u-picker
          :show="show1"
          :columns="columns"
          keyName="label"
          @change="changeHandler"
          @confirm="confirm"
          @cancel="show1 = false"></u-picker>
        <!-- <selfPicker level="1" :defaultIds="communityIdx" @change="communitychange" idKey="id" name="name"
          :columnData="communityData"></selfPicker> -->

        <u-icon slot="right" name="arrow-right"></u-icon>
      </u-form-item>

      <u-form-item
        @click="xiaoqu"
        label="所在小区"
        prop="community"
        borderBottom
        ref="community"
        v-if="community_dictTexts">
        <u--input
          v-model="community_dictText"
          disabled
          disabledColor="#ffffff"
          placeholder="请选择小区"
          border="none">
        </u--input>

        <u-icon slot="right" name="arrow-right"></u-icon>
      </u-form-item>

      <u-form-item
        label="所在楼栋"
        prop="floor"
        borderBottom
        ref="floor"
        @click="ld"
        v-if="floordictTexts">
        <u--input
          v-model="form.ld"
          disabled
          disabledColor="#ffffff"
          placeholder="请选择楼栋"
          border="none">
        </u--input>
        <u-picker
          :show="show3"
          :columns="columns2"
          keyName="label"
          @confirm="confirm2"
          @cancel="show3 = false">
        </u-picker>
        <!-- <selfPicker level="1" :defaultIds="floorIdx" @change="floorchange" idKey="id" name="floornumber"
          :columnData="floorData"></selfPicker> -->
        <u-icon slot="right" name="arrow-right"></u-icon>
      </u-form-item>

      <u-form-item
        label="所在楼栋"
        prop="floor"
        borderBottom
        ref="floor"
        @click="loudong"
        v-if="floor_dictTexts">
        <u--input
          v-model="floor_dictText"
          disabled
          disabledColor="#ffffff"
          placeholder="请选择楼栋"
          border="none">
        </u--input>
        <u-icon slot="right" name="arrow-right"></u-icon>
      </u-form-item>

      <u-form-item
        label="所在单元"
        prop="unit"
        borderBottom
        ref="unit"
        @click="dy"
        v-if="unitdictTexts">
        <u--input
          v-model="form.dy"
          disabled
          disabledColor="#ffffff"
          placeholder="请选择单元"
          border="none"
          @click="show2 = true">
        </u--input>
        <u-picker
          :show="show2"
          :columns="columns1"
          keyName="unitnumber"
          @confirm="confirm1"
          @cancel="show2 = false"></u-picker>
        <!-- <selfPicker level="1" :defaultIds="unitIdx" @change="unitchange" idKey="id" name="unitnumber"
          :columnData="unitData"></selfPicker> -->
        <u-icon slot="right" name="arrow-right"></u-icon>
      </u-form-item>

      <u-form-item
        label="所在单元"
        prop="unit"
        borderBottom
        ref="unit"
        @click="danyuan"
        v-if="unit_dictTexts">
        <u--input
          v-model="unit_dictText"
          disabled
          disabledColor="#ffffff"
          placeholder="请选择单元"
          border="none"
          @click="show2 = true">
        </u--input>

        <u-icon slot="right" name="arrow-right"></u-icon>
      </u-form-item>

      <u-form-item
        label="所在房号"
        prop="number"
        borderBottom
        ref="number"
        required>
        <u--input
          type="number"
          v-model="form.number"
          border="none"
          placeholder="请输入所在房号"
          clearable>
        </u--input>
      </u-form-item>

      <view class="margin-top-lg">
        <u-button
          type="primary"
          :loading="btnLoading"
          text="提交"
          @click="submit()">
        </u-button>
      </view>
    </u--form>

    <u-action-sheet
      :show="showgender"
      :actions="genderActions"
      title="请选择性别"
      @close="showgender = false"
      @select="genderSelect">
    </u-action-sheet>
    <!--       <u-picker immediateChange :show="showRegion" ref="uPicker" :columns="regionColumns" :defaultIndex="regionIndex"
            @confirm="regionConfirm" @change="regionChangeHandler" keyName="name" @cancel="showRegion=false">
        </u-picker> -->

    <!--        <view v-if="current==3" class="u-success-panel">
                <view class="u-success-panel__circle">
                    <u-icon name="checkbox-mark" color="#fff" size="30"></u-icon>
                </view>
                <text class="u-success-panel__normal">已完成认证</text>
            </view> -->
  </view>
</template>

<script>
import selfPicker from '@/components/picker.vue'
import { mapState } from 'vuex'
import { community } from '@/common/api.js'
const defaultForm = {
  name: '',
  gender: '',
  gender_text: '',
  region: '',
  region_json: null,
  community: '',
  floor: '',
  unit: '',
  number: '',
  xq: '',
  ld: '',
  dy: '',
  community_xq: '',
  unit_dy: '',
  floor_ld: '',
  dq: '',
}
export default {
  components: {
    selfPicker,
  },
  filters: {},
  props: ['details'],
  data() {
    return {
      fileList1: [],
      floor_dictTexts: false,
      floordictTexts: false,
      community_dictTexts: false,
      communitydictTexts: false,
      unit_dictTexts: false,
      unitdictTexts: false,
      unit_dictText: '',
      community_dictText: '',
      floor_dictText: '',
      city: false,
      citys: false,
      show1: false,
      columns: [],
      show2: false,
      show3: false,
      columns1: [],
      columns2: [],
      globalData: getApp().globalData,
      btnLoading: false,
      pageLoading: true,
      showgender: false,
      genderActions: [
        {
          name: '男',
          value: 1,
        },
        {
          name: '女',
          value: 2,
        },
      ],

      regionIdx: [],
      communityIdx: [],
      floorIdx: [],
      unitIdx: [],

      communityData: [],

      form: uni.$u.deepClone(defaultForm),
      rules: {
        nickName: {
          type: 'string',
          required: true,
          message: '请填写姓名',
          trigger: ['blur', 'change'],
        },
        name: [
          {
            type: 'string',
            required: true,
            message: '请填写姓名',
            trigger: ['blur', 'change'],
          },
          {
            // 此为同步验证，可以直接返回true或者false，如果是异步验证，稍微不同，见下方说明
            validator: (rule, value, callback) => {
              // 调用uView自带的js验证规则，详见：https://www.uviewui.com/js/test.html
              return uni.$u.test.chinese(value)
            },
            message: '姓名必须为中文',
            // 触发器可以同时用blur和change，二者之间用英文逗号隔开
            trigger: ['change', 'blur'],
          },
        ],
        gender: {
          type: 'number',
          required: true,
          message: '请选择男或女',
          trigger: ['blur', 'change'],
        },
        phone: [
          {
            required: true,
            message: '请填写联系电话',
            trigger: ['change', 'blur'],
          },
          {
            // 自定义验证函数，见上说明
            validator: (rule, value, callback) => {
              // 上面有说，返回true表示校验通过，返回false表示不通过
              // uni.$u.test.mobile()就是返回true或者false的
              return uni.$u.test.mobile(value)
            },
            message: '手机号码不正确',
            // 触发器可以同时用blur和change
            trigger: ['change', 'blur'],
          },
        ],
        region: {
          type: 'number',
          required: true,
          message: '请选择所在地区',
          trigger: ['change'],
        },
        // community: {
        //     type: "integer",

        //     message: '请选择所在小区',
        //     trigger: ['change']
        // },
        // floor: {
        //     type: "integer",

        //     message: '请选择所在楼栋',
        //     trigger: ['change']
        // },
        // unit: {
        //     type: "integer",

        //     message: '请选择所在单元',
        //     trigger: ['change']
        // },
        number: [
          {
            required: true,
            message: '请填写房号',
            trigger: ['change', 'blur'],
          },
          {
            // 自定义验证函数，见上说明
            validator: (rule, value, callback) => {
              // 上面有说，返回true表示校验通过，返回false表示不通过
              // uni.$u.test.mobile()就是返回true或者false的
              if (value <= 0) {
                return false
              }
              return uni.$u.test.digits(value)
            },
            message: '请输入整数',
            // 触发器可以同时用blur和change
            trigger: ['change', 'blur'],
          },
        ],
      },
    }
  },
  computed: {
    ...mapState({
      user: state => state.user,
      cityDataTree: state => state.cityDataTree,
    }),
    // floorData() {
    //   if (this.form.community == '' || this.communityData.length <= 0) {
    //     return []
    //   }
    //   const communityIndex = this.communityData.findIndex(item => {
    //     if (item.id == this.form.community) {
    //       return true;
    //     }
    //     return false;
    //   })
    //   if (communityIndex >= 0) {
    //     return this.communityData[communityIndex].floor;
    //   } else {
    //     return []
    //   }
    // },
    // unitData() {
    //   if (this.form.floor == '' || this.floorData.length <= 0) {
    //     return []
    //   } else {
    //     //设置单元数据
    //     const floorIndex = this.floorData.findIndex(item => {
    //       if (item.id == this.form.floor) {
    //         return true;
    //       }
    //       return false;
    //     })
    //     if (floorIndex >= 0) {
    //       return this.floorData[floorIndex].unit;
    //     } else {
    //       return []
    //     }

    //   }
    // }
  },
  watch: {
    pageLoading(val) {
      if (!val) {
        this.$nextTick(() => {
          this.$refs['form'] && this.$refs['form'].setRules(this.rules)
        })
      }
    },
  },
  mounted() {
    this.$refs['form'] && this.$refs['form'].setRules(this.rules)

    // this.form = this.details

    // name: ''
    // this.form.gender = this.details.gender
    // this.form.gender_text = this.details.gender == 0 ? '男' : '女'
    // this.form.region = this.details.region
    // this.form.region_json = this.details
    // this.form.community = this.details
    // this.form.floor = this.details
    // this.form.unit = this.details
    // number: ''
  },
  created() {
    const userid = uni.getStorageSync('users').id
    uni.request({
      // url: `https://wj.api.cestech.com.cn:48002/mission_b/v1/user/info?id=${userid}`,
      url: `https://wj.api.cestech.com.cn:48002/mission_b/v1/user/info`,
      header: {
        'x-Resource-Token': uni.getStorageSync('xtoken'),
        //'x-Resource-Token':uni.getStorageSync('xtoken'), //自定义请求头信息
      },
      // url: `https://wj.api.cestech.com.cn:48002/mission_b/v1/user/info`,
      // data:{
      // 	id:userid,
      // 	// pageNo:1,
      // 	// pageSize:10
      // },
      method: 'GET',
      success: res => {
        console.log(res.data.result, 111)
        // this.$store.commit('refreshUser',
        //   data)
        this.details = res.data.result.records[0]
        this.form = this.details
        this.community_dictText = res.data.result.records[0].community_dictText
        this.floor_dictText = res.data.result.records[0].floor_dictText
        this.unit_dictText = res.data.result.records[0].unit_dictText
        if (res.data.result.records[0].unit_dictText) {
          this.unit_dictTexts = true
          this.unitdictTexts = false
        } else {
          this.unitdictTexts = true
          this.unit_dictTexts = false
        }

        if (res.data.result.records[0].community_dictText) {
          this.community_dictTexts = true
          this.communitydictTexts = false
        } else {
          this.communitydictTexts = true
          this.community_dictTexts = false
        }

        if (res.data.result.records[0].floor_dictText) {
          this.floor_dictTexts = true
          this.floordictText = false
        } else {
          this.floordictTexts = true
          this.floor_dictTexts = false
        }
        //回显地区
        if (this.details.regionJson) {
          this.regionIdx = JSON.parse(this.details.regionJson).map(item => {
            console.log(item.value, 'xxx')
            return item.value
          })

          console.log(this.regionIdx, this.cityDataTree)
        }

        this.form.gender_text = this.details.gender == 0 ? '男' : '女'
        // this.community(this.details.region, true)
        if (res.data.result.regionJson) {
          this.citys = true
          this.city = false
          let datas = []
          console.log(JSON.parse(res.data.result.regionJson), '地区')
          JSON.parse(res.data.result.regionJson).map(v => {
            console.log(v.name)
            datas.push(v.name)
          })
          console.log(datas)
          this.form.dq = datas + ''
        } else {
          this.city = true
        }
      },
    })
    if (this.details.reg_status) {
      console.log(222)
      this.form = this.details
      //回显地区
      if (this.details.region_json) {
        this.regionIdx = this.details.region_json.map(item => {
          return item.value
        })
      }

      this.form.gender_text =
        this.details.gender == 2 ? '女' : this.details.gender == 1 ? '男' : ''
      // this.community(this.details.region, true)

      // this.communityIdx = [this.form.community]
      // this.floorIdx = [this.form.floor]
      // this.unitIdx = [this.form.unit]
    } else {
      this.pageLoading = false
    }
  },
  onShow() {},
  methods: {
    avaUrl() {
      uni.chooseImage({
        // 从本地相册选择图片或使用相机拍照。
        count: 1, //默认选择1张图片
        sizeType: ['original', 'compressed'], //original 原图，compressed 压缩图，默认二者都有
        success: res => {
          console.log(res.tempFilePaths[0]) //成功则返回图片的本地文件路径列表 tempFilePaths
          this.form.avatarUrl = res.tempFilePaths[0] //更新本低浏览头像图片
          this.update(res.tempFilePaths[0]) //上传图片
        },
      })
    },
    //上传图片
    update(filePath) {
      const _this = this
      const token = uni.getStorageSync('xtoken')
      const res = uni.uploadFile({
        url: 'https://wj.api.cestech.com.cn:48002/mission_b/wx/WxApi/imgUpload', //仅为示例，非真实的接口地址
        filePath: filePath,
        name: 'file',
        header: {
          'x-Resource-Token': uni.getStorageSync('xtoken'),
        },
        success: res => {
          console.log(JSON.parse(res.data).result, 'toux')
          this.form.avatarUrl = JSON.parse(res.data).result
          // console.log(JSON.parse(res.data).data)  //处理格式
          // this.weChatRegister(JSON.parse(res.data).result)	//将真实图片地址请求给后端做修改
        },
      })
    },

    xiaoqu() {
      this.community_dictTexts = false
      this.communitydictTexts = true
      this.xq()
      // this.show1=true
    },
    loudong() {
      this.floor_dictTexts = false
      this.floordictTexts = true
      this.ld()
    },
    danyuan() {
      this.unit_dictTexts = false
      this.unitdictTexts = true
      this.dy()
    },
    city_s() {
      this.citys = false
      this.city = true
    },
    xq() {
      console.log(this.columns, '000')
      this.columns = []
      this.show1 = true
      uni.request({
        // url: 'https://wj.api.cestech.com.cn:48002/mission_b/v1/sy/cultura/listbycustomer',
        url: `https://wj.api.cestech.com.cn:48002/mission_b/v1/supervisor/dist/task/list3`,
        header: {
          'x-Resource-Token': uni.getStorageSync('xtoken'),
          //'x-Resource-Token':uni.getStorageSync('xtoken'), //自定义请求头信息
        },
        method: 'GET',
        success: res => {
          // console.log(res,'小区')
          this.columns = []
          let xq = []
          let data = []
          // xq.push()
          res.data.result.map(item => {
            let obj = {
              label: item.namne,
              id: item.id,
            }
            this.columns = []
            data.push(obj)
            this.columns.push(data)
          })
          console.log(this.columns, 'this.columns2')
        },
      })
    },
    ld() {
      if (this.form.community_xq) {
        this.columns2 = []

        uni.request({
          // url: 'https://wj.api.cestech.com.cn:48002/mission_b/v1/sy/cultura/listbycustomer',
          url: `https://wj.api.cestech.com.cn:48002/mission_b/v1/supervisor/dist/task/list?communityId=${this.form.community_xq}`,
          header: {
            'x-Resource-Token': uni.getStorageSync('xtoken'),
            //'x-Resource-Token':uni.getStorageSync('xtoken'), //自定义请求头信息
          },
          method: 'GET',
          success: res => {
            // console.log(res,'小区')

            //小区楼栋
            let xq_ld = []
            let data_dy = []
            xq_ld.push(res.data.result.ld[0].ld)
            xq_ld.map(item => {
              let obj = {
                label: item.floornumber,
                id: item.id,
              }
              data_dy.push(obj)
              this.columns2.push(data_dy)
            })

            console.log(this.columns2, 'this.columns2')
            this.show3 = true
          },
        })
      } else {
        uni.showToast({
          icon: 'none',
          title: '请先选择小区在选择楼栋',
        })
      }
    },
    dy() {
      if (this.form.community_xq && this.form.floor_ld) {
        this.columns3 = []

        uni.request({
          // url: 'https://wj.api.cestech.com.cn:48002/mission_b/v1/sy/cultura/listbycustomer',
          url: `https://wj.api.cestech.com.cn:48002/mission_b/v1/supervisor/dist/task/list?communityId=${this.form.community_xq}`,
          header: {
            'x-Resource-Token': uni.getStorageSync('xtoken'),
            //'x-Resource-Token':uni.getStorageSync('xtoken'), //自定义请求头信息
          },
          method: 'GET',
          success: res => {
            // console.log(res,'小区')

            this.show2 = true
            // // 小区单元
            let data_ld = []
            res.data.result.ld[0].dy.map(item => {
              let obj = {
                unitnumber: item.unitnumber,
                id: item.id,
              }

              data_ld.push(obj)
            })
            this.columns1.push(data_ld)
          },
        })
      } else {
        uni.showToast({
          icon: 'none',
          title: '请先选择小区和楼栋',
        })
      }
    },
    confirm(e) {
      console.log(e.value)
      this.form.xq = e.value[0].label
      this.form.community_xq = e.value[0].id
      this.show1 = false
    },
    confirm1(e) {
      console.log(e.value)
      this.form.dy = e.value[0].unitnumber
      this.form.unit_dy = e.value[0].id
      this.show2 = false
    },
    confirm2(e) {
      console.log(e.value)
      this.form.ld = e.value[0].label
      this.form.floor_ld = e.value[0].id
      this.show3 = false
    },

    // community(val, status = false) {
    //   community({
    //     region: val
    //   }).then((res) => {
    //     this.communityData = res.data
    //     this.pageLoading = false;
    //   })

    //   uni.request({
    //     url: `https://wj.api.cestech.com.cn:48002/mission_b/wx/WxApi/community?id=${10}`,
    //     method: "POST",
    //     success: (res) => {
    //       // this.item.booked = res.
    //       // this.communityData = res.data.result
    //       // console.log(this.communityData);
    //       this.communityIdx = [res.data.result[0]['小区'].id]
    //       console.log(this.communityIdx);
    //       this.floorIdx = [this.communityData.floor]
    //       this.unitIdx = [this.communityData.unit]
    //     }

    //   })
    // },
    genderSelect(e) {
      this.form.gender = e.value
      this.form.gender_text = e.name
      this.$refs.form.validateField('gender')
    },
    // 地区选择变化
    regionChange(e) {
      this.form.region = e.value[e.value.length - 1]
      this.regionIdx = e.detail.map(item => item.value)
      this.form.region_json = e.detail
      this.$refs.form.validateField('region')
      this.resetCommunity()
      this.community(this.form.region)
    },
    resetRegion() {
      this.form.region = ''
      this.regionIdx = []
    },
    communitychange(e) {
      this.form.community = e.value[0]
      this.communityIdx = [e.value[0]]
      console.log(this.communityIdx)
      this.$refs.form.validateField('community')
      this.resetFloor()
    },
    resetCommunity() {
      this.form.community = ''
      this.communityIdx = []
      this.resetFloor()
      this.$refs.form.validateField('community')
    },
    floorchange(e) {
      this.form.floor = e.value[0]
      this.floorIdx = [e.value[0]]
      this.$refs.form.validateField('floor')
      this.resetUnit()
    },
    resetFloor() {
      this.form.floor = ''
      this.floorIdx = []
      this.resetUnit()

      this.$refs.form.validateField('floor')
    },
    unitchange(e) {
      this.form.unit = e.value[0]
      this.unitIdx = [e.value[0]]
      this.$refs.form.validateField('unit')
    },
    resetUnit() {
      this.form.unit = ''
      this.unitIdx = []
      this.$refs.form.validateField('unit')
    },
    submit() {
      let pages = getCurrentPages() // 当前页面
      let beforePage = pages[pages.length - 2]
      // 如果有错误，会在catch中返回报错信息数组，校验通过则在then中返回true
      this.$refs.form
        .validate()
        .then(res => {
          const params = {
            avatarUrl: this.form.avatarUrl,
            nickName: this.form.nickName,
            name: this.form.name,
            region: this.form.region,
            regionJson: JSON.stringify(this.form.region_json),
            gender: this.form.gender,
            community: this.form.community,
            floor: this.form.floor,
            unit: this.form.unit,
            number: this.form.number,
            community: this.form.community_xq,
            floor: this.form.floor_ld,
            unit: this.form.unit_dy,
          }
          this.$emit('success', params)
          setTimeout(() => {
            uni.navigateBack({
              delta: 1,
              success: function () {
                beforePage.$vm.init() // 执行前一个页面的init 来实现刷新
              },
            })
            // uni.navigateBack()
            // uni.navigateTo({
            // 	url:`/pageD/address/index?fig=1`
            // })
          }, 300)
        })
        .catch(errors => {})
    },
  },
}
</script>

<style lang="scss" scoped></style>
