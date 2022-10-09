<template>

  <view class="content">
    <!-- 遮罩层 用来跳转到商品详情页 -->
     <view class="bg"  @click="gotoDetail(goods)"></view>
    <view class="goods-item" >
      <!-- 商品左侧图片区域 -->
      <view class="goods-item-left">
        <radio class="radio" :checked="goods.goods_state" color="#C00000" v-if="showRadio" @click="radioClickHandler"></radio>
        <image class="goods-pic" :src="goods.goods_small_logo || defaultPic" mode="" ></image>
      </view>
      <!-- 商品右侧信息区域 -->
      <view class="goods-item-right">
        <!-- 商品标题 -->
        <view class="goods-name" >{{goods.goods_name}}</view>
        <view class="goods-info-box">
          <!-- 商品价格 -->
          <view class="goods-price">￥{{goods.goods_price| tofixed}}</view>
          <!-- 商品数量 -->
          <uni-number-box :min="1" :value="goods.goods_count" @change="numChangeHandler"  v-if="showNum"></uni-number-box>
        </view>
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
      // 是否展示图片左侧的 radio
      showRadio: {
        type: Boolean,
        // 如果外界没有指定 show-radio 属性的值，则默认不展示 radio 组件
        default: false,
      },
      // 是否展示价格右侧的 NumberBox 组件
      showNum: {
        type: Boolean,
        default: false,
      },


    },

    data() {
      return {

        // 默认的图片，防止图片缺失时，该位置没有图片
        defaultPic: 'https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg12.360buyimg.com%2Fn12%2Fs400x400_jfs%2Ft1%2F154854%2F6%2F21178%2F141503%2F603f4372E81ffc866%2Fa94c81730a3ae289.jpg&refer=http%3A%2F%2Fimg12.360buyimg.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1667003644&t=0bb61d95af1cb63ead3fcf7247535c0a'
      }
    },
    methods: {
      // radio 组件的点击事件处理函数
      radioClickHandler() {
        // 商品的 goods_id  商品最新的勾选状态goods_state
        this.$emit('radio-change', {
          goods_id: this.goods.goods_id,
          goods_state: !this.goods.goods_state
        })
      },
      // NumberBox 组件的 change 事件处理函数
      numChangeHandler(val){
        this.$emit('num-change',{
          // 商品的 Id
          goods_id: this.goods.goods_id,
          // 商品的最新数量
          goods_count: +val
        })
      },
      // 点击跳转到商品详情页面
      gotoDetail(goods) {
        uni.navigateTo({
           url:'/subpkg/goods_detail/goods_detail?goods_id=' + goods.goods_id
        })
      }
    },
    filters: {
      // 
      tofixed(num) {
        // 把数字处理为带两位小数点的数字
        return Number(num).toFixed(2)
      }
    }

  }
</script>
<style lang="scss">
  .content{
    position: relative;
    .goods-item {
      // 让 goods-item 项占满整个屏幕的宽度
      width: 750rpx;
      // 设置盒模型为 border-box
      box-sizing: border-box;
      display: flex;
      padding: 10px 5px;
      border-bottom: 1px solid #f0f0f0;
    
      .goods-item-left {
        margin-right: 5px;
        display: flex;
        justify-content: space-between;
        align-items: center;
       .radio{
         z-index: 4;
       }
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
        uni-number-box{
          z-index: 4;
        }
      }
    }
    // 遮罩层 
    .bg{
      bottom: 0;
      position:absolute;
      // background-color: pink;
      z-index: 2;
      width: 750rpx;
      height: 100%;
      opacity: 10%;
    }
  }
  
</style>
