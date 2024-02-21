<!-- 查看视频 -->
<template>
	<view class="container">
		<u-loading-page :loading="pageLoading"></u-loading-page>
		<view v-if="!pageLoading">
			<view class="z-form-text u-p">
				<view class="z-form-item">
					<view class="z-form-item__body">
						<view class="z-form-item__body__left">
							<u--text text="活动标题:"></u--text>
						</view>
						<view class="z-form-item__body__right">
							<u--text wordWrap="anywhere" :text="detail.name">
							</u--text>

						</view>
					</view>
					<u-line color="rgb(214, 215, 217)"></u-line>
				</view>


				<view class="z-form-item">
					<view class="z-form-item__body">
						<view class="z-form-item__body__left">
							<u--text text="活动类型:"></u--text>
						</view>
						<view class="z-form-item__body__right">
							<u--text wordWrap="anywhere" :text="detail.type|activityTypeText">
							</u--text>

						</view>
					</view>
					<u-line color="rgb(214, 215, 217)"></u-line>
				</view>

				<view class="z-form-item">
					<view class="z-form-item__body">
						<view class="z-form-item__body__left">
							<u--text text="开始时间:"></u--text>
						</view>
						<view class="z-form-item__body__right">
							<u--text wordWrap="anywhere" :text="detail.startDate">
							</u--text>

						</view>
					</view>
					<u-line color="rgb(214, 215, 217)"></u-line>
				</view>

				<view class="z-form-item">
					<view class="z-form-item__body">
						<view class="z-form-item__body__left">
							<u--text text="活动时长:"></u--text>
						</view>
						<view class="z-form-item__body__right">
							<u--text wordWrap="anywhere" :text="detail.activityTime">
							</u--text>

						</view>
					</view>
					<u-line color="rgb(214, 215, 217)"></u-line>
				</view>

				<view class="z-form-item">
					<view class="z-form-item__body">
						<view class="z-form-item__body__left">
							<u--text text="活动负责人:"></u--text>
						</view>
						<view class="z-form-item__body__right">
							<u--text wordWrap="anywhere" :text="speaker? speaker.name :''">
							</u--text>

						</view>
					</view>
					<u-line color="rgb(214, 215, 217)"></u-line>
				</view>

				<view class="z-form-item">
					<view class="z-form-item__body">
						<view class="z-form-item__body__left">
							<u--text text="活动地址:"></u--text>
						</view>
						<view class="z-form-item__body__right">
							<u--text wordWrap="anywhere" :text="detail.activitySite">
							</u--text>

						</view>
					</view>
					<u-line color="rgb(214, 215, 217)"></u-line>
				</view>

				<view class="z-form-item">
					<view class="z-form-item__body_col">
						<view class="z-form-item__body__top">
							<u--text text="备注:"></u--text>
						</view>
						<view class="z-form-item__body__right">
							<u--text wordWrap="anywhere" :text="detail.remark">
							</u--text>

						</view>
					</view>
				</view>
			</view>
		</view>
	</view>


	</view>
</template>

<script>
	import {
		activityone
	} from '@/common/api.js'


	export default {
		components: {

		},
		data() {
			return {
				speaker: '',
				pageLoading: true,
				globalData: getApp().globalData,
				id: null,
				detail: {
					name_of_activity: '',
					type: '',
					create_time: '',
					start_date: '',


				}
			}
		},
		onLoad(option) {
			if (option.id) {
				this.id = option.id
				this.getDetail();
			} else {
				uni.$u.route({
					type: 'navigateBack'

				})
				return
			}

		},
		onUnload() {

		},
		computed: {},

		methods: {
			getDetail() {
				this.pageLoading = true;
				// activityone({
				//   id: this.id
				// }).then((res) => {
				//   this.detail = res.data
				//   this.pageLoading = false;

				// })

				uni.request({
					url: `https://wj.api.cestech.com.cn:48002/mission_b/v1/2/activity/one?id=${this.id}`,
					header: {
						'x-Resource-Token':uni.getStorageSync('xtoken'),
						//'x-Resource-Token':uni.getStorageSync('xtoken'), //自定义请求头信息
					},
					// data: {
					//   pageNo: page,
					//   pageSize: list_rows,
					//   customer_id: 1,
					// },
					method: "GET",
					success: (res) => {
						console.log(res.data, 'xxxx')
						this.pageLoading = false;
						this.detail = res.data.result.activity
						this.speaker = res.data.result.Speaker
						// this.$refs.paging.complete(res.data.result.records);
						// res.data.result.records.forEach(v => {
						//   console.log(v.points, 'vvvv')
						//   // v.knowledge["points"] = v.points;
						//   // console.log(v,'我是v')
						// })
						// this.$refs.paging.complete(res.data.result.records);
						// console.log(res.data.result.records, 'res.data.data')

					}

				})

			},

		}
	}
</script>

</style>
<style lang="scss" scoped>
	page {
		background-color: #fff;
	}
</style>