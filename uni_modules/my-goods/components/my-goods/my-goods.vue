<template>
    
        <view class="goods-item">
          <!-- 商品左侧图片区域 -->
          <view class="goods-item-left">
            <image class="goods-pic" :src="goods.goods_small_logo || defaultPic" mode=""></image>
          </view>
          <!-- 商品右侧信息区域 -->
          <view class="goods-item-right">
            <!-- 商品标题 -->
            <view class="goods-name">{{goods.goods_name}}</view>
            <view class="goods-info-box">
              <!-- 商品价格 -->
              <view class="goods-price">￥{{goods.goods_price| tofixed}}</view>
            </view>
          </view>
        </view>
      
    
 
</template>
<script>
  export default {
    // 定义 props 属性，用来接收外界传递到当前组件的数据
    props: {
      // 商品的信息对象
      goods: {
        type: Object,
        defaul: {},
      },
    },
    data() {
      return {
       
        // 默认的图片，防止图片缺失时，该位置没有图片
        defaultPic: 'https://tuchuangs.com/imgs/2022/09/14/3191d4c8d9f04efb.webp'
      }
    },
    filters:{
      // 
      tofixed(num){
        // 把数字处理为带两位小数点的数字
        return Number(num).toFixed(2)
      }
    }
    
  }
</script>
<style lang="scss">

    .goods-item {
      display: flex;
      padding: 10px 5px;
      border-bottom: 1px solid #f0f0f0;
  
      .goods-item-left {
        margin-right: 5px;
  
        .goods-pic {
          width: 100px;
          height: 100px;
          display: block;
        }
      }
  
      .goods-item-right {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
  
        .goods-name {
          font-size: 13px;
        }
        .goods-price {
        font-size: 16px;
        color: #c00000;
        }
  
      }
    }
  
  
</style>
