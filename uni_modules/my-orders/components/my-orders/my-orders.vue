<template>
  <view class="content">
    <view class="order-item">
      <!-- 订单号数-->
      <view class="order-title">
        <view class="order-num">
          订单号：{{orders.order_number}}
        </view>
        <view class="">
          待付款
        </view>
      </view>
      <!-- 主体 -->
      <view class="order-body" v-for="(item2,index2) in orders.goods" :key="index2">
        <view class="goods-item">
          <!-- 商品左侧图片区域 -->
          <view class="goods-item-left">
            <image class="goods-pic" :src="item2.goods_logo || defaultPic" mode="" ></image>
          </view>
          <!-- 商品右侧信息区域 -->
          <view class="goods-item-right">
            <!-- 商品标题 -->
            <view class="goods-name" >{{item2.goods_name}}</view>
            <view class="goods-info-box">
              <!-- 商品价格 -->
              <view class="goods-price">￥{{item2.goods_price| tofixed}}</view>
              <view class="goods_number">X {{item2.goods_number}}</view>
            </view>
          </view>
        </view>
        
      </view>
      <!-- 底部时间区域 -->
      <view class="botton-box">
        <!-- 时间 -->
        <view class="order-time">{{orders.create_time | formatDate}}</view>
        <!-- 共几件 -->
        <view class="total-order">
          <view class="order-num">共计{{orders.checkedCount}}件</view>
          <view class="goods-price">合计￥{{orders.order_price}}</view>
        </view>
      </view>
    </view>
           
  </view>
</template>
<script>
  export default{
    data(){
      return{
        
      }
    },
    props:{
      orders:{
          type: Object,
          defaul: {},
        }
    },
    onLoad() {
      // this.a()
    },
    filters: {
      // 
      tofixed(num) {
        // 把数字处理为带两位小数点的数字
        return Number(num).toFixed(2)
      },
      formatDate(value) {
      		if(value == undefined){
      			return;
      		}
      		// let date = new Date(value * 1000);
      		let date = new Date(value * 1000);
      		//时间戳为10位需*1000，时间戳为13位的话不需乘1000
      		let y = date.getFullYear();
      		let MM = date.getMonth() + 1;
      		MM = MM < 10 ? ('0' + MM) : MM; //月补0
      		let d = date.getDate();
      		d = d < 10 ? ('0' + d) : d; //天补0
      		let h = date.getHours();
      		h = h < 10 ? ('0' + h) : h; //小时补0
      		let m = date.getMinutes();
      		m = m < 10 ? ('0' + m) : m; //分钟补0
      		let s = date.getSeconds();
      		s = s < 10 ? ('0' + s) : s; //秒补0
      		// return y + '-' + MM + '-' + d; //年月日
      		return y + '-' + MM + '-' + d + ' ' + h + ':' + m+ ':' + s; //年月日时分秒
      	}
     
    },
    methods:{
      // a(){
      //   console.log(orders);
      // }
    }
  }
</script>
<style lang="scss">
  .content{
    // 订单项
    .order-item {
      // width: 100%;
      align-items: center;
      border-radius: 10px;
      background-color: white;
      justify-content: space-around;
      font-size: 13px;
      padding: 10px 5px;
      // 订单号数
      .order-title {
        width: 100%;
        display: flex;
        justify-content: space-between;
        margin-right: 5px;
      }
      // 主体
      .order-body {
        margin-top: 10rpx;
        // background-color: #fff;
        .goods-item {
          // 让 goods-item 项占满整个屏幕的宽度
          // width: 750rpx;
          // 设置盒模型为 border-box
          box-sizing: border-box;
          display: flex;
          padding: 10px 0;
          border-bottom: 1px solid #f0f0f0;
        
          .goods-item-left {
            margin-right: 5px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            .goods-pic {
              width: 100px;
              height: 100px;
              display: block;
            }
          }
        
          .goods-item-right {
            display: flex;
            flex: 1;
            flex-direction: column;
            justify-content: space-between;
        
            .goods-name {
              font-size: 13px;
            }
        
            .goods-info-box {
              display: flex;
              align-items: center;
              justify-content: space-between;
            }
        
            .goods-price {
              font-size: 16px;
              color: #c00000;
            }
          
          }
        }
      }
    }
    .botton-box{
      padding: 10px 5px;
      display: flex;
      flex-direction: column;
      // justify-content: space-between;
      // align-items: center;
      .order-time{
        margin: 10px 0;
        font-size: 14px
      }
      .total-order{
        display: flex;
        justify-content: space-between;
        .order-num{
          font-size: 14px;
          // color: #afafaf;
        }
        .goods-price{
          font-size: 16px;
          color: #c00000;
        }
      }
    }
  }
</style>
