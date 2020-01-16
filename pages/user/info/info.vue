<template>
	<view>
		<cmd-page-body type="top">
			<cmd-transition name="fade-up">
				<view>
					<cmd-cel-item title="头像" slot-right>
						<cmd-avatar :src="user.icon"></cmd-avatar>
					</cmd-cel-item>
					<cmd-cel-item title="昵称" :addon="user.nickname" ></cmd-cel-item>
					<cmd-cel-item title="姓名" :addon="user.name" ></cmd-cel-item>
					<cmd-cel-item title="毕业年度" :addon="user.name" ></cmd-cel-item>
					<cmd-cel-item title="性别" :addon="user.name" ></cmd-cel-item>
					<cmd-cel-item title="邮箱" @click="fnClick('email')" :addon="user.email" arrow></cmd-cel-item>
					<cmd-cel-item title="联系方式" @click="fnClick('phone')" :addon="user.phone" arrow></cmd-cel-item>
					<cmd-cel-item title="通讯地址" @click="fnClick('address')" :addon="user.address" arrow></cmd-cel-item>
					<cmd-cel-item title="修改密码" @click="fnClick('modify')" arrow></cmd-cel-item>
					<button class="btn-logout">退出登录</button>
				</view>

				<!-- 城市选择弹框 -->
				<mpvue-city-picker :themeColor="themeColor" ref="mpvueCityPicker" :pickerValueDefault="cityPickerValueDefault"
				 @onCancel="onCancel" @onConfirm="onConfirm"></mpvue-city-picker>
				<!-- 输入框 -->
				<uni-popup :show="inputonshow" :custom="true" :mask-click="false">
					<view class="uni-tip">
						<view class="uni-tip-title">{{inputTitle}}</view>
						<input :type="inputType" focus="true" adjust-position v-model="inputModel" :maxlength="inputMaxlength" />
						<view class="uni-tip-group-button">
							<view class="uni-tip-button" @click="cancel(falseCancleType)">取消</view>
							<view class="uni-tip-button" @click="cancel(trueCancleType)">确定</view>
						</view>
					</view>
				</uni-popup>
			</cmd-transition>
		</cmd-page-body>
	</view>
</template>

<script>
	import uniPopup from '@/components/uni-popup/uni-popup.vue'
	import mpvueCityPicker from '../../../components/mpvue-citypicker/mpvueCityPicker.vue'
	import cityData from '../../../common/city.data.js'
	import cmdNavBar from "@/components/cmd-nav-bar/cmd-nav-bar.vue"
	import cmdPageBody from "@/components/cmd-page-body/cmd-page-body.vue"
	import cmdTransition from "@/components/cmd-transition/cmd-transition.vue"
	import cmdCelItem from "@/components/cmd-cell-item/cmd-cell-item.vue"
	import cmdAvatar from "@/components/cmd-avatar/cmd-avatar.vue"

	export default {
		components: {
			uniPopup,
			mpvueCityPicker,
			cmdNavBar,
			cmdPageBody,
			cmdTransition,
			cmdCelItem,
			cmdAvatar
		},

		data() {
			return {
				content: "",
				type: "",
				phoneReg: /^[1](([3][0-9])|([4][5-9])|([5][0-3,5-9])|([6][5,6])|([7][0-8])|([8][0-9])|([9][1,8,9]))[0-9]{8}$/,
				emailReg:/\w[-\w.+]*@([A-Za-z0-9][-A-Za-z0-9]+\.)+[A-Za-z]{2,14}/,
				user: {
					nickname: "昵称",
					name: "名字",					
					email:"2366573897@qq.com",
					phone: "13612447497",
					avatar: "https://avatar.bbs.miui.com/images/noavatar_small.gif",
					address: "请选择地址"

				},
				cityPickerValueDefault: [0, 0, 0],
				inputTemp: "",
				inputonshow: false,
				inputTitle:"请输入手机号",
				inputType:"text",
				inputMaxlength:"11",
				falseCancleType:"phonefalse",
				trueCancleType:"phonetrue",
				inputModel:""
			};
		},

		mounted() {},

		methods: {
			onLoad(event) {
				this.user = JSON.parse(decodeURIComponent(event.user))

			},
			togglePopup(type, open) {
				this.content = '底部弹出 popup'

				this.type = type,
					this.$refs[open].open()
			},
			change(e) {
				console.log(e.show)
			},
			/**
			 * 点击触发
			 * @param {Object} type 跳转页面名或者类型方式
			 */
			fnClick(type) {
				if (type == 'modify') {
					uni.navigateTo({
						url: '/pages/user/modify/modify'
					})
				} else if (type == 'address') {
					this.$refs.mpvueCityPicker.show()
				} else if (type == 'phone') {
					this.inputonshow = true
					this.inputTitle='请输入手机号'
					this.inputType='number'
					this.inputMaxlength='11'
					this.falseCancleType='phonefalse'
					this.trueCancleType='phonetrue'
					this.inputModel=this.user.phone
				}else if (type == 'email') {
					this.inputonshow = true
					this.inputTitle='请输入邮箱'
					this.inputType='text'
					this.inputMaxlength='-1'
					this.falseCancleType='emailfalse'
					this.trueCancleType='emailtrue'
					this.inputModel=this.user.email
				}
			},
			cancel(type) {
				if (type === 'phonefalse') {
					this.inputonshow = false
					return
				} else if (type === 'phonetrue') {
					if (!this.phoneReg.test(this.inputModel)) {
						uni.showToast({
							title: "请输入有效的手机号码",
							icon: "none"
						})
					}else{
						this.user.phone=this.inputModel
					}
					this.inputonshow = false
					return
				}else if (type === 'emailfalse') {
					this.inputonshow = false
					return
				} else if (type === 'emailtrue') {
					if (!this.emailReg.test(this.inputModel)) {						
						uni.showToast({
							title: "请输入有效的电子邮箱",
							icon: "none"
						})
					}else{
						this.user.email = this.inputModel
					}
					this.inputonshow = false
					return
				}
				
				
				this.$refs[type].close()
			},

			onConfirm(e) {
				this.user.address = e.label
			},
			onCancel(e) {
				console.log(e)
			},
		}
	}
</script>

<style >
	.btn-logout {
		margin-top: 100upx;
		width: 80%;
		border-radius: 50upx;
		font-size: 16px;
		color: #fff;
		background: linear-gradient(to right, #365fff, #36bbff);
	}

	.btn-logout-hover {
		background: linear-gradient(to right, #365fdd, #36bbfa);
	}

	/* 提示窗口 */
	.uni-tip {
		padding: 15px;
		width: 300px;
		background: #fff;
		box-sizing: border-box;
		border-radius: 10px;
	}

	.uni-tip-title {
		text-align: center;
		font-weight: bold;
		font-size: 16px;
		color: #333;
	}

	.uni-tip-content {
		padding: 15px;
		font-size: 14px;
		color: #666;
	}

	.uni-tip-group-button {
		margin-top: 10px;
		display: flex;
	}

	.uni-tip-button {
		width: 100%;
		text-align: center;
		font-size: 14px;
		color: #3b4144;
	}
</style>
