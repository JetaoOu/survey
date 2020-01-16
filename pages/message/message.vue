v<template>
	<view>
		<view class="text-area">
			<view class="uni-list">
				<view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(value, key) in listData" :key="key" @click="drumpTO(value.status,value.questionnairenumber,value.questionnaireid)">
					<view class="uni-media-list">
						<image class="uni-media-list-logo" :src="value.image"></image>
						<view class="uni-media-list-body">
							<view class="uni-media-list-text-top">{{ value.title }}</view>
							<view class="uni-media-list-text-bottom">

								<text>{{ value.author }}</text>
								<text>{{ value.changetime }}</text>
							</view>
						</view>
					</view>
				</view>
			</view>
			<uni-load-more :status="status" :content-text="contentText" />
		</view>
	</view>
</template>

<script>
	import uniLoadMore from '@/components/uni-load-more/uni-load-more.vue';
	export default {
		data() {
			return {
				requestUrl: "",
				logined: true,
				status: 'more',
				title: 'Message。。。。。。。。。。。',
				listData: []
			}
		},
		methods: {
			drumpTO(status, questionnairenumber, questionnaireId) {
				var that = this
				if (!getApp().globalData.logined) {
					uni.navigateTo({
						url:"../login/login"
					})
				} else {
					if (status == 2) {
						uni.navigateTo({
							url: '/pages/questionnaire-detail/questionnaire-detail?questionnairenumber=' + questionnairenumber +
								"&questionnaireId=" + questionnaireId
						})
					} else if (status == 3) {
						uni.showToast({
							icon: "none",
							title: "问卷已经截止！"
						})
					}
				}


			},
			//查询数据库，用户是否已经注册过
			getUser() {
				let data;
				uni.request({
					url: 'http://' + this.requestUrl + '/user/findOne.action?id=' + getApp().globalData.wxopenid,
					success: data => {
						console.log("getuser======================")
						if (data.statusCode == 200) {
							console.log(JSON.stringify(data))
							if (data.data=="") {
								uni.navigateTo({
									url: "../regist/regist"
								})								
							}else{
								this.user = data.data
								getApp().globalData.user = data.data
								getApp().globalData.hasRegist = true
								console.log(getApp().globalData.hasRegist)
								uni.switchTab({
									url: '../message/message'
								})
							}
			
						}
					},
					fail: (data, code) => {
						console.log('getUserFail' + JSON.stringify(data));
					}
				})
			},
			// 获取消息
			getListData() {
				uni.request({
					url: 'http://' + this.requestUrl + '/questionnaire/findAll.action',
					success: data => {
						if (data.statusCode == 200) {
							this.listData = data.data
						}
					},
					fail: (data, code) => {
						console.log('getlistdata fail    ' + JSON.stringify(data));
					}
				})
			}

		},
		onShow() {
			getApp().globalData.user.icon=getApp().globalData.userInfo.avatarUrl
			getApp().globalData.user.nickname=getApp().globalData.userInfo.nickName
			this.logined = getApp().globalData.logined
			this.getListData()
		},
		onLoad() {			
			this.requestUrl = getApp().globalData.requestUrl
			this.getListData()
		},
		onPullDownRefresh() {
			console.log('refresh');
			this.getListData()
			setTimeout(function() {
				uni.stopPullDownRefresh();
			}, 1000);
		}

	}
</script>

<style scoped>

</style>
