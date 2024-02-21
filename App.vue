<script>
import authorize from '@/common/auth.js'
var QQMapWX = require('@/common/qqmap-wx-jssdk.min.js')

var staticUrl = 'http://cdn.cestech.com.cn/'
export default {
  globalData: {
    systemName: '垃圾分类宣教宝',
    systemKey: 'slaughter',
    baseurl: 'http://sport-manage.api.9dyf.cn',
    qqmapsdk: null,
    locationKey: 'WTYBZ-JZPC2-D2QUB-CRBNE-NXHZ6-NWBLL',
    background: {
      backgroundColor: '#fff',
      // 导航栏背景图
      // background: 'url(https://cdn.uviewui.com/uview/swiper/1.jpg) no-repeat',
      // 还可以设置背景图size属性
      // backgroundSize: 'cover',

      // 渐变色
      // backgroundImage: 'linear-gradient(45deg, rgb(28, 187, 180), rgb(141, 198, 63))'
    },
    theme: {
      '$u-main-color': '#303133',
      '$u-content-color': '#606266',
      '$u-tips-color': '#909193',
      '$u-light-color': '#c0c4cc',
      '$u-border-color': '#dadbde',
      '$u-bg-color': '#f3f4f6',
      '$u-disabled-color': '#c8c9cc',
      '$u-primary': '#3BCF99 ',
      '$u-primary-dark': '#26c6da',
      '$u-primary-disabled': '#80deea',
      '$u-primary-light': '#b2ebf2',
      '$u-warning': '#f9ae3d',
      '$u-warning-dark': '#f1a532',
      '$u-warning-disabled': '#f9d39b',
      '$u-warning-light': '#fdf6ec',
      '$u-success': '#5ac725',
      '$u-success-dark': '#53c21d',
      '$u-success-disabled': '#a9e08f',
      '$u-success-light': '#f5fff0',
      '$u-error': '#f56c6c',
      '$u-error-dark': '#e45656',
      '$u-error-disabled': '#f7b2b2',
      '$u-error-light': '#fef0f0',
      '$u-info': '#909399',
      '$u-info-dark': '#767a82',
      '$u-info-disabled': '#c4c6c9',
      '$u-info-light': '#f4f4f5',
      '$theme-background-color': '#fff',
    },
    hasScopeUserInfo: false,
    staticUrl: staticUrl,
    // 首页写死数据
    index: {
      menuList: [
        {
          id: '1559063181911056386',
          url: 'http://cdn.cestech.com.cn/BkpKnowledge/img/a411ae8815d7430f9d6c5a306057e2a3.png',
          title: '分类基础',
          children: [],
        },
        {
          id: '1559063685168816129',
          url: 'http://cdn.cestech.com.cn/BkpKnowledge/img/76718b2645af4db98c8755445bf139a1.png',
          title: '行业资讯',
          children: [],
        },

        {
          id: '1559063685168816129',
          url: 'http://cdn.cestech.com.cn/BkpKnowledge/img/9d8c0b0b632f4c57adcee3f35eba1460.png',
          title: '专题报告',
          children: [
            {
              id: '1572846998715031553',
              url: staticUrl + 'static/special1.png',
              title: '限塑令',
            },
            {
              id: '1572847071544926210',
              url: staticUrl + 'static/special2.png',
              title: '光盘行动',
            },
            {
              id: '1572847137819123714',
              url: staticUrl + 'static/special3.png',
              title: '低碳出行',
              children: [],
            },
            {
              id: '1572847168588537857',
              url: staticUrl + 'static/special4.png',
              title: '过度包装',
              children: [],
            },
          ],
        },
        {
          id: '1559063181911056386',
          url: 'http://cdn.cestech.com.cn/TrainingProgram/img/5621a70946714a70a76104407474383b.png',
          title: '我要预约',
          route: '/pageB/myreservation/index',
        },
        {
          id: '1559063181911056386',
          url: 'http://cdn.cestech.com.cn/BkpKnowledge/img/6f7d47862ffc443790879e0276c4c121.png',
          title: '我要打卡',
          route: '/pageD/checkType/checkType',
        },
        {
          id: '1559063685168816111',
          url: 'http://cdn.cestech.com.cn/BkpKnowledge/img/1d49caec1ae6442eb95e381a2ceed970.png',
          // url: 'http://cdn.cestech.com.cn/BkpKnowledge/img/06a2231956574d5c9c98cc795da48148.png',
          title: '分类查询',
          route: 'pageB/classification/classification',
        },
        // {
        //   url: '/static/assets/home/icon4.png',
        //   title: '活动管理',
        //   route: 'pageA/activity/index'

        // },
      ],
      current: 0,
      swiperList: [
        {
          id: '1562993914442604545',
          url: 'https://oms.cestech.com.cn/BkpKnowledge/img/5c2f5388fe49417c995fa10b75e2b1d5/007@2x.jpg',
          // url: staticUrl + 'static/banner1.png'
        },
        {
          id: '1562981491304226818',
          url: staticUrl + 'static/banner2.png',
        },
        {
          id: '1560557218266537986',
          url: 'https://oms.cestech.com.cn/BkpKnowledge/img/5d00a076c1c34913a5e2ceeb97f04589/004@2x.jpg',
          // url: staticUrl + 'static/banner3.jpg'
        },
      ],
      list1: [
        //      {
        // 	id: '1564891172083785729',
        // 	url: `${staticUrl}BkpKnowledge/img/f15f423111f546d9b0424f3ea19dc144.jpg'`,
        // 	title: '生态环境保护这十年，生态篇',
        // 	type: '行业资讯'

        // },
        {
          id: '1664523524482633730',
          url: `${staticUrl}BkpKnowledge/img/f15f423111f546d9b0424f3ea19dc144.jpg'`,
          title: '2023年双流区东升街道生活垃圾分类 示范社区/小区创建',
          type: '行业资讯',
        },
        {
          id: '1560166099775053826',
          url: `${staticUrl}BkpKnowledge/img/888aefacbe134ee3b465405dea50bcc9.jpg`,
          title: '2025年底前，安庆实现城区生活垃圾分类全覆盖',
          type: '专题报告',
        },
      ],
      list2: [
        {
          id: '1564889207111081985',
          url: `${staticUrl}BkpKnowledge/img/47d2c6c34d6740549099e8838b4d32ca.jpg`,
          title:
            '外卖塑料处理成难题！互联网平台外卖塑料包装五年增长为原来的10倍',
          type: '专题报告',
        },
        {
          id: '1561611412993925121',
          url: `${staticUrl}BkpKnowledge/img/93d6a4d15b644795a668d2f0a6b10f1a.jpg`,
          title: '亚洲最大垃圾协同焚烧处置单体项目获批',
          type: '专题报告',
        },
      ],
    },
  },
  onLaunch: function () {
    this.globalData.qqmapsdk = new QQMapWX({
      key: this.globalData.locationKey,
    })
    // if(uni.getStorageSync('avatarUrl')){
    // this.login()
    // }
  },

  onPageNotFound: function () {
    // 跳转到 404 页面：
    uni.$u.route({
      url: 'pages/error/404',
    })
  },
  onHide: function () {},
  methods: {
    // 获取客户地区列表
    getUserRegion() {
      this.$store.commit('setRegions')
      this.$store.commit('setDefaultRegion')
    },
    async login() {
      await authorize.login()
      // const launch = uni.getLaunchOptionsSync()
      // const noLaunch = ["pages/education/detail"]

      // if (noLaunch.indexOf(launch.path) == -1) {
      //     this.checkAuth()
      // }
      // this.checkAuth()
      this.getUserRegion()
    },
    checkAuth() {
      if (
        !this.$store.state.token ||
        !this.$store.state.user.phone ||
        !this.$store.state.user.reg_status
      ) {
        this.$u.route({
          type: 'navigateTo',
          url: 'pageC/auth/register',
        })
        return
      }
    },
  },
}
</script>

<style lang="scss">
@import 'colorui/main.css';
@import 'colorui/icon.css';
/*每个页面公共css */
@import '@/uni_modules/uview-ui/index.scss';
@import 'common/styles.scss';
</style>
