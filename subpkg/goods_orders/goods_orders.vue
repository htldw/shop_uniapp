<template>
  <view class="content">
   <view class="content-order">
     <!-- 顶部区域 -->
     <view class="top-box">
       <view class="top"  v-for="(item,index) in arr" :key="index">
         <view :class="['top-item', index===active?'active':'']" @click="activeChanged(index)" >{{item.name}}</view>
       </view>
     </view>
      <!-- 订单列表区域 -->
      <view class="order-list"  v-for="(item1,index) in goods_order" :key="index1">
          <my-orders :orders="item1"></my-orders>
      </view>
      
   </view>
  </view>
</template>

<script>
  // 按需导入 mapState 这个辅助函数
  import {
    mapState,
    mapMutations
  } from 'vuex'
  export default {
    data() {
      return {
        arr:[{
          name:'全部'
        },{
          name:'待付款'
        },{
          name:'待收款'
        },{
          name:'退款/退货'
        }
        ],
        // timer:this.goods_order,
        active:0
      };
    },
    onLoad:function(){
       //  this.goods_order.forEach((item,index)=>{    //js遍历数组
       //      this.goods_order[index].create_time = this.happenTimeFun(this.goods_order[index].create_time);
      	// });
    		// (3)
       // clearInterval(this.countdownTimer); //进来前先清除一遍定时器
       // this.setTimer();
        // this.a()
    	},
      // (2)
      // destroyed: function() {
      // 			// 每次离开当前界面时，清除定时器
      // 			clearInterval(this.countdownTimer)
      // 			this.countdownTimer = null;
      // 		},
    methods:{
      a(){
        console.log(this.goods_order);
      
      },
      activeChanged(index){
       this.active = index;
      },
       // happenTimeFun(num){//时间戳数据处理
       //       // let num1 = num * 1000;
       //       let date = new Date(num*1000);
      	// 		 //时间戳为10位需*1000，时间戳为13位的话不需乘1000
      	// 		 let y = date.getFullYear();
      	// 		 let MM = date.getMonth() + 1;
      	// 		 MM = MM < 10 ? ('0' + MM) : MM;//月补0
      	// 		 let d = date.getDate();
      	// 		 d = d < 10 ? ('0' + d) : d;//天补0
      	// 		 let h = date.getHours();
      	// 		 h = h < 10 ? ('0' + h) : h;//小时补0
      	// 		 let m = date.getMinutes();
      	// 		 m = m < 10 ? ('0' + m) : m;//分钟补0
      	// 		 let s = date.getSeconds();
      	// 		 s = s < 10 ? ('0' + s) : s;//秒补0
      	// 		 return y + '-' + MM + '-' + d; //年月日
      	// 		 //return y + '-' + MM + '-' + d + ' ' + h + ':' + m+ ':' + s; //年月日时分秒
      	// 	 },

      // (1)
     //  setTimer() {
     //    const num = this.goods_order[0].create_time
     //  				if (this.countdownTimer == null) {
     //  					this.countdownTimer = setInterval(() => {
     //  						this.countdown();
     //  					}, 1000)
     //  				}
     //  			},
     //  			countdown() {
     //  				let date = new Date();
     //  				const yearNow = date.getFullYear();
     //  				const monthNow = Number(date.getMonth()) + 1;
     //  				const dayNow = date.getDate();
     //  				let end = yearNow + "-" + monthNow + "-" + dayNow + " " + "23:59:59";
     //  				end = new Date((end).replace(/-/g,'/')).getTime();
     //  				let start = new Date(date.toString().replace(/-/g,'/')).getTime();
     //  				let surplus = (end - start).toFixed(0); //剩余毫秒数
       
     //  				let hours = parseInt(surplus / 1000 / 60 / 60 % 24, 10); //计算剩余的小时
     //  				let minutes = parseInt(surplus / 1000 / 60 % 60, 10); //计算剩余的分钟
     //  				let seconds = parseInt(surplus / 1000 % 60, 10); //计算剩余的秒数
     //  				String(hours).length == 1 ? this.hours = "0" + String(hours) : this.hours = String(hours);
     //  				String(minutes).length == 1 ? this.minutes = "0" + String(minutes) : this.minutes = String(minutes);
     //  				String(seconds).length == 1 ? this.seconds = "0" + String(seconds) : this.seconds = String(seconds);
     //  			},
     
    },
    
    computed:{
      // 将 m_cart 模块中的 cart 数组映射到当前页面中使用
      ...mapState('m_cart', ['goods_order']),
    }
  }
</script>

<style lang="scss">
  page,
  .content {
    // padding: 10rpx;
    height: 100%;
    background-color: #f4f4f4;
    .content-order{
      .top-box {
        background-color: white;
        width: 100%;
        height: 100rpx;
        display: flex;
        flex-direction: row;
        justify-content: space-around;
        align-items: center;
        border-bottom: 1px solid white;
        margin-bottom: 10rpx;
        .top{
          width: 100%;
          .top-item{
              // 激活项的样式
              width: 100%;
              // background-color: #c00000;
              height: 100rpx;
              line-height: 100rpx;
              text-align: center;
              &.active{
                border-bottom: 3px solid #c00000;
              }
              
            }
          }
        }
        // margin: ;
       
      // 订单列表区域
      .order-list {
          margin: 20rpx;
          height: 100%;
          display: flex;
          flex-direction: column;
        // height: 100%;
        // background-color: gray;
       
      }
    }
   
  }
</style>
