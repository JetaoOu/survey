<template>
	
	<view v-if="onlogin">
		<cmd-page-body type="top">
			<cmd-transition name="fade-up">
				<view>
					<cmd-cel-item title="头像" slot-right>
						<cmd-avatar :src="user.icon"></cmd-avatar>
					</cmd-cel-item>
					<cmd-cel-item title="昵称" :addon="user.nickname"></cmd-cel-item>
					<cmd-cel-item title="姓名" :addon="user.name"></cmd-cel-item>
					<cmd-cel-item title="联系方式" :addon="user.phone"></cmd-cel-item>
					<cmd-cel-item title="我的地址" :addon="user.address"></cmd-cel-item> 
					<cmd-cel-item title="信息设置" @click="fnClick('info')" arrow></cmd-cel-item>

				</view>

			</cmd-transition>
		</cmd-page-body>
	</view>
	<view v-else>
		<button type="primary" @click="login">登陆</button>		
	</view>
</template>

<script>
	import cmdNavBar from "@/components/cmd-nav-bar/cmd-nav-bar.vue"
	import cmdPageBody from "@/components/cmd-page-body/cmd-page-body.vue"
	import cmdTransition from "@/components/cmd-transition/cmd-transition.vue"
	import cmdCelItem from "@/components/cmd-cell-item/cmd-cell-item.vue"
	import cmdAvatar from "@/components/cmd-avatar/cmd-avatar.vue"

	export default {
		components: {
			cmdNavBar,
			cmdPageBody,
			cmdTransition,
			cmdCelItem,
			cmdAvatar
		},

		data() {
			return {
				onlogin:true,
				user: getApp().globalData.user
			};
		},

		onLoad() {
			this.onlogin=getApp().globalData.logined
		},

		methods: {
			/**
			 * 点击触发
			 * @param {Object} type 跳转页面名或者类型方式
			 */ 
			fnClick(type) {
				if (type == 'info') {
					uni.navigateTo({
						url: '/pages/user/info/info?user='+encodeURIComponent(JSON.stringify(this.user))
					})
				}
			},
			login(){
				uni.navigateTo({
					url:"../login/login"
				})
			}

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
</style>
