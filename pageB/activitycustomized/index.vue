<template>
	<view>
		<view class="u-nav">
		  <navbar :safeAreaInsetTop="true" height="55" placeholder leftIconColor="#fff" autoBack>
		    <view class="u-nav-slot" slot="center">
		      <u--text text="活动定制" color="#fff"></u--text>
		    </view>
		  </navbar>
		</view>
		<view class="list">
			<view class="li" v-for="item in list" @click="dateli(item.name)">
				<view class="img">
					<image :src="item.url" style="width: 100%;height: 100%;" mode="aspectFill"></image>
				</view>
				<view>{{item.name}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [{
						url: '../../static/assets/home/dz.png',
						name: '大型活动'
					},
					{
						url: '../../static/assets/home/dz.png',
						name: '中型活动'
					},
					{
						url: '../../static/assets/home/dz.png',
						name: '小型活动'
					}
				],
				num: 0
			}
		},
		onLoad() {
			if(!uni.getStorageSync('phone')){
				uni.showToast({
					title:"请登录后再进行操作",
					icon:"none"
				})
				setTimeout(()=>{
					uni.switchTab({
						url:'/pages/center/index'
					})
				},1000)
			}
		},
		methods: {
			dateli(val) {
				console.log(val)
				if (val === '大型活动') {
					this.num = 0
				} else if (val === '中型活动') {
					this.num = 1
				} else {
					this.num = 2
				}
				uni.navigateTo({
					url:`/pageB/activitycustomized/details?num=${this.num}`
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.list {
		display: flex;
		align-items: center;
		justify-content: space-between;
		margin-top: 20rpx;

		.li {
			width: 33.333%;
			text-align: center;
			font-size: 30rpx;
			font-weight: 500;

			.img {
				width: 120rpx;
				height: 120rpx;
				margin: 0 auto;
				margin-bottom: 20rpx;
			}
		}
	}
</style>
