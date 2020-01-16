<template>
	<view>
		<view class="example-title">专业核心能力级学习成效满意度</view>
		<view class="example-info"><text>{{foreword}}</text></view>
		<view class="example-info"><text>请您为以下选项打分，1星为无关联，5星为非常关联</text></view>
		<form @submit="formSubmit($event,form)" @reset="formReset">
		<view class="example-body">
			<view v-for="item in list" :key="item.id" class="example-box">
				<uni-card :title="item.title" is-shadow="true" :note="item.note">
					<view>{{item.content}}</view>					
					<template v-slot:footer>
						<view>
							<view class="">{{question1}}</view>
							<sunui-star :defaultStar="0" isTips="true" :maxStar="5" @changeStar="changeStar($event,item.id)" class='sunui-star'></sunui-star>
						</view>
						</br>
						<view>
							<view class="">{{question2}}</view>
							<sunui-star :defaultStar="0" isTips="true" :maxStar="5"  @changeStar="changeStar($event,item.id)" class='sunui-star'></sunui-star>
						</view>
					</template>
				</uni-card>
			</view>
		</view>
		<view class="uni-btn-v">
				<button formType="submit">Submit</button>
				<button type="default" formType="reset">Reset</button>
			</view>
		</form>
	</view>
</template>

<script>
	import uniCard from '@/components/uni-card/uni-card.vue'
	import sunuiStar from '@/components/sunui-star/sunui-star.vue'
	export default {
		components: {
			uniCard,
			sunuiStar
		},
		data() {
			return {
				title: "应届毕业生问卷调查表",
				foreword: "请问您就读本专业时，本专业课程的设计与教师教学是否提供您具备下列各项核心能力？",
				question1: '您认为此项核心能力对于您毕业后就业的重要性',
				question2: '您认为您在学习本专业是是否已教授您获得此项核心能力',
				list: [{
					id: 0,
					title: '1.人文社会科学素养、良好职业规范：',
					content: '具有人文社会科学素养，社会责任感强，能够在计算机应用工程实践中理解并遵守工程职业道德和规范，履行责任。',
					shadow: true,
					note: 'Tips',
					extra: '额外信息',
					thumbnail: 'https://img-cdn-qiniu.dcloud.net.cn/new-page/uni.png'
				},{
					id: 1,
					title: '2.人文社会科学素养、良好职业规范：',
					content: '具有人文社会科学素养，社会责任感强，能够在计算机应用工程实践中理解并遵守工程职业道德和规范，履行责任。',
					shadow: true,
					note: 'Tips',
					extra: '额外信息',
					thumbnail: 'https://img-cdn-qiniu.dcloud.net.cn/new-page/uni.png'
				},{
					id: 2,
					title: '3.人文社会科学素养、良好职业规范：',
					content: '具有人文社会科学素养，社会责任感强，能够在计算机应用工程实践中理解并遵守工程职业道德和规范，履行责任。',
					shadow: true,
					note: 'Tips',
					extra: '额外信息',
					thumbnail: 'https://img-cdn-qiniu.dcloud.net.cn/new-page/uni.png'
				}],
				//onload
				questionnaireId:"",
				requestUrl:'',
				form: {
					title: "问卷标题",
					list: []
				},
				ee: {},
				ans: []
			}
		},
		onLoad(e) {
			uni.setNavigationBarTitle({
				title: this.title
			})
			this.questionnaireId=e.questionnaireId
			this.requestUrl = getApp().globalData.requestUrl
			let questionnairenumber = e.questionnairenumber
			// 请求问卷数据
			uni.request({
				   url: 'http://'+this.requestUrl+'/questiongroup/findByNum.action?num=' +
					questionnairenumber.toString(),
				method: 'GET',
				data: {},
				success: res => {
					this.form.list = res.data
					this.ee = this.form.list[0].questionList[1].optionsList
					for (let i = 0; i < res.data.length; i++) {
						this.ans.push({
							values: []
						})
					}
				},
				fail: () => {},
				complete: () => {}
			});
		},
		methods: {
			formSubmit(e, form) {
				console.log("e: " + JSON.stringify(e));
				console.log("form: " + JSON.stringify(form));
			},
			formReset() {
				console.log("formReset")
			},
			changeStar(e,value){
				console.log("e: " + JSON.stringify(e));
				console.log("valu: " + value);
			},
			clickCard() {
				uni.showToast({
					title: '点击卡片',
					icon: 'none'
				})
			},
			footerClick(types) {
				uni.showToast({
					title: types,
					icon: 'none'
				})
			}
		}
	}
</script>

<style >
	page {
		display: flex;
		flex-direction: column;
		box-sizing: border-box;
		background-color: #efeff4
	}

	view {
		font-size: 28upx;
		line-height: inherit
	}
	

	.example {
		padding: 0 30upx 30upx
	}

	.example-title {
		display: flex;
		justify-content: space-between;
		align-items: center;
		font-size: 32upx;
		color: #464e52;
		padding: 30upx 30upx 30upx 50upx;
		margin-top: 20upx;
		position: relative;
		background-color: #fdfdfd;
		border-bottom: 1px #f5f5f5 solid
	}

	.example-title__after {
		position: relative;
		color: #031e3c
	}

	.example-title:after {
		content: '';
		position: absolute;
		left: 30upx;
		margin: auto;
		top: 0;
		bottom: 0;
		width: 6upx;
		height: 32upx;
		background-color: #ccc
	}

	.example .example-title {
		margin: 40upx 0
	}

	.example-body {
		padding: 30upx;
		background: #fff
	}

	.example-info {
		padding: 30upx;
		color: #3b4144;
		background: #fff
	}

	.example-body {
		padding: 30upx 0;
	}

	.example-box {
		margin-bottom: 30upx;
	}

	.example-box:last-child {
		margin-bottom: 0;
	}

	.image-box {
		width: 100%;
		height: 350upx;
		overflow: hidden;
	}

	.image-box .image {
		width: 100%;
		height: 100%;
	}

	.content-box {
		padding-top: 20upx;
	}

	.footer-box {
		display: flex;
		justify-content: space-between;
		width: 100%;

	}

	.footer-box__item {
		width: 100%;
		display: flex;
		align-items: center;
	}

	.footer-box__item:nth-child(2) {
		justify-content: center;
	}

	.footer-box__item:last-child {
		justify-content: flex-end;
	}
</style>
