<template>
  <view class="pay-content">
    <view class="top-box">
      <my-address></my-address>
    </view>
    <view class="order-box">
      <my-orders :orders="goods_order[0]"></my-orders>
    </view>
    <!-- 按钮结算区域 -->
    <view class="pay-btn">
     <button type="primary" class="btn-pay"  @click="payOrder(payInfo)">立即支付</button>
    </view>
  </view>
</template>

<script>
  //   按需导入 mapGetters 这个辅助函数
  import {
    mapGetters,
    // mapMutations,
    mapState
  } from 'vuex'
  export default {
    data() {
      return {
        payInfo:{},
        
      };
    },
    onLoad() {
     
    },
    methods:{
      // 支付订单按钮
      async payOrder(){
       // console.log(this.payInfo);
       // 3. 发起微信支付
       // 3.1 调用 uni.requestPayment() 发起微信支付
       const [err, succ] = await uni.requestPayment(this.payInfo)
       // console.log(err);
       // 3.2 未完成支付
       if (err) return uni.$showMsg('订单无权限支付！')
       // 3.3 完成了支付，进一步查询支付的结果
       const { data: res3 } = awaituni.$http.post('/api/public/v1/my/orders/chkOrder', { 
           order_number:orderNumber 
       })
       // 3.4 检测到订单未支付
       if (res3.meta.status !== 200) return uni.$showMsg('订单未支付！')
       // 3.5 检测到订单支付完成
       uni.showToast({
         title: '支付完成！',
         icon: 'success'
       })
      }
    },
    computed:{
      // 将 m_cart 模块中的 cart 数组映射到当前页面中使用
      ...mapState('m_cart', ['goods_order']),
    }
  }
</script>

<style lang="scss">
  .pay-content{
    position: relative;
    .top-box{
      pointer-events:none;
    }
    .order-box{
      border-bottom: 1px solid #f0f0f0;
    }
    .pay-btn{
      height: 70px;
      // 登录按钮的样式
      .btn-pay {
        position: fixed;
        bottom: 10px;
        left: 18px;
        width: 90%;
        border-radius: 100px;
        margin: 0 auto;
        background-color: #c00000;
      }
    }
     
  }
  
</style>
