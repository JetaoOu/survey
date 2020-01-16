<script>
	export default {
		globalData: {
			user:{
				"icon":"https://img.36krcdn.com/20191219/v2_2bd0177b86be491fbd92adb2310e2998_img_png",
				"nickname":"小杰",
				"address":"广东省-潮州市",
				"name":"欧杰涛",
				"phone":"13612447498"
			},
			userInfo:{},
			wxopenid: '9352bb6555ce45e7aaf7b5480a80c846',
			logined:true,
			hasRegist:false,
			requestUrl:"www.waixingren.online"//"192.168.137.1:8080/myproject-questionnaire"
		},
		onLaunch: function() {
			console.log('App Launch');

		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	@import './common/uni.css';
</style>
