<script>

	export default {
 
		onLaunch: function() {
			// console.warn('当前组件仅支持 uni_modules 目录结构 ，请升级 HBuilderX 到 3.1.0 版本以上！')
			// console.log('App Launch')
		},
		onShow: function() {
			// console.log('App Show')
		},
		onHide: function() {
			// console.log('App Hide')
		}
	}
</script>

<style lang="scss">
  @font-face {
    font-family: "iconfont";
    /* Project id 3172664 */
    src: 
      url('//at.alicdn.com/t/font_3172664_imrn5un97fr.woff2?t=1645370942110') format('woff2'),
      url('//at.alicdn.com/t/font_3172664_imrn5un97fr.woff?t=1645370942110') format('woff'),
      url('//at.alicdn.com/t/font_3172664_imrn5un97fr.ttf?t=1645370942110') format('truetype');
  }
	/*每个页面公共css */
	@import '@/uni_modules/uni-scss/index.scss';
	/* #ifndef APP-NVUE */
	@import '@/static/customicons.css';
	// 设置整个项目的背景色
	page {
		background-color: #ffffff;
	}

	/* #endif */
	.example-info {
		font-size: 14px;
		color: #333;
		padding: 10px;
	}
</style>
