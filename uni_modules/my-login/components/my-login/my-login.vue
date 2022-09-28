<template>
	<view class="login-container">
	    <!-- 提示登录的图标 -->
	    <uni-icons type="contact-filled" size="100" color="#AFAFAF"></uni-icons>
	    <!-- 登录按钮 -->
	    <button type="primary" class="btn-login"  @click="getUserProfile">一键登录</button>
	    <!-- 登录提示 -->
	    <view class="tips-text">登录后尽享更多权益</view>
	  </view>
</template>
<script>
	import {mapState,mapMutations } from 'vuex'
	export default{
		data(){
			return{
				token:'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWQiOjIzLCJpYXQiOjE1NjQ3MzAwNzksImV4cCI6MTAwMTU2NDczMDA3OH0.YPt-XeLnjV-_1ITaXGY2FhxmCe4NvXuRnRB8OMCfnPo '
			}
		},
		methods:{
			...mapMutations('m_user',['updateUserInfo','updateToken','updateRedirectInfo']),
			// 获取微信用户的基本信息
			 async getUserProfile() {
				 const res = await uni.getUserProfile({
				 	desc:'登录授权',
					  success: (res) => {
						  // console.log(res);
							// 3. 将用户的基本信息存储到 vuex 中
							this.updateUserInfo(res.userInfo)
							 // 获取登录成功后的 Token 字符串
							this.getToken(res)
						  },
						  fail: (res) => {
							return uni.$showMsg('您取消了登录授权')
						  }
				 })
			  },
			  async getToken(info){
				const [err,res] = await uni.login().catch(err => err)
				// console.log(res);
				 // 判断是否 uni.login() 调用失败
				 if(err||res.errMsg !== 'login:ok') return uni.$showError('登录失败！')
				 // 用户基本信息
				 // const query = {
					//  code : res.code,
					//  encryptedData: info.encryptedData,
					//  iv:info.iv,
					//  rawData:info.rawData,
					//  signature:info.signature
				 // }
				  // 换取 token
				 // const {data:loginResult } = await uni.$http.post('/api/public/v1/users/wxlogin',query)
				 // const msg = loginResult.message || {}
         // console.log(loginResult);
				 // if (!(loginResult.meta.status === 200||loginResult.meta.status === 400)) return uni.$showMsg('登录失败！')
         // 生成随机token
				 // for(var i=0;i<=32 ;i++){
					//  const n = Math.floor(Math.random()*16.0).toString(16)
					//  msg.token += n
				 // }
				// console.log(msg);
				// 2. 更新 vuex 中的 token,截取前面几个字符串
				this.updateToken(this.token)
				
				// 判断 vuex 中的 redirectInfo 是否为 null
				// 如果不为 null，则登录成功之后，需要重新导航到对应的页面
				this.navigateBack()
			  },
			  // 返回登录之前的页面
			  navigateBack(){
				  // redirectInfo 不为 null，并且导航方式为 switchTab
				  // console.log(this);
				  if(this.redirectInfo && this.redirectInfo.openType === 'switchTab'){
					  // 调用小程序提供的 uni.switchTab() API 进行页面的导航
					  uni.switchTab({
					  	url:this.redirectInfo.from,
						// 导航成功之后，把 vuex 中的 redirectInfo 对象重置为 null
						complete: () => {
							this.updateRedirectInfo(null)
						}
					  })
				  }
			  }
		},
		computed:{
			 // 调用 mapState 辅助方法，把 m_user 模块中的数据映射到当前用组件中使用
			...mapState('m_user', ['redirectInfo']),
		}
	}
</script>
<style lang="scss">
	.login-container {
	  // 登录盒子的样式
	  height: 750rpx;
	  display: flex;
	  flex-direction: column;
	  align-items: center;
	  justify-content: center;
	  background-color: #f8f8f8;
	  position: relative;
	  overflow: hidden;
	
	  // 绘制登录盒子底部的半椭圆造型
	  &::after {
	    content: ' ';
	    display: block;
	    position: absolute;
	    width: 100%;
	    height: 40px;
	    left: 0;
	    bottom: 0;
	    background-color: white;
	    border-radius: 100%;
	    transform: translateY(50%);
	  }
	
	  // 登录按钮的样式
	  .btn-login {
	    width: 90%;
	    border-radius: 100px;
	    margin: 15px 0;
	    background-color: #c00000;
	  }
	
	  // 按钮下方提示消息的样式
	  .tips-text {
	    font-size: 12px;
	    color: gray;
	  }
	}
</style>
