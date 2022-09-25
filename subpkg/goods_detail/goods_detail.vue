<template>
  <view class="goods-detail" v-if="goods_info.goods_name">

    <!-- 轮播图 -->
    <swiper v-if="goods_info.pics.length!==0" :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000"
      circular="true">
      <swiper-item v-for="(item,index) in goods_info.pics" :key="index">
        <!-- 把当前点击的图片的索引，传递到 preview() 处理函数中 -->
        <image :src="item.pics_big" mode="" @click="preview(index)"></image>
      </swiper-item>
    </swiper>
    <!-- 没有轮播图的详情页 暂时用这个代替 -->
    <swiper v-else :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" circular="true">
      <swiper-item v-for="(item2,index2) in defaultPic" :key="index2">
        <image :src="item2.url" mode="" @click="preview(index)"></image>
      </swiper-item>
    </swiper>
    <!-- 商品信息区域 -->
    <view class="goods-info-box">
      <!-- 商品价格 -->
      <view class="price">￥{{goods_info.goods_price}}</view>
      <view class="goods-info-body">
        <!-- 	商品名称 -->
        <view class="goods-name">{{goods_info.goods_name}}</view>
        <!-- 收藏 -->
        <view class="favi">
          <uni-icons type="star" size="18" color="gray"></uni-icons>
           <text>收藏</text>
        </view>
      </view>
      <view class="yf">
        快递: 免运费
      </view>
    </view>
    <!-- 商品详情信息 -->
    <rich-text :nodes="goods_info.goods_introduce"></rich-text>
    <!-- 商品导航组件 -->
    <view class="goods_nav">
       
      <!-- fill 控制右侧按钮的样式 -->
       
      <!-- options 左侧按钮的配置项 -->
       
      <!-- buttonGroup 右侧按钮的配置项 -->
       
      <!-- click 左侧按钮的点击事件处理函数 -->
       
      <!-- buttonClick 右侧按钮的点击事件处理函数 -->
       
      <uni-goods-nav :fill="true" :options="options" :buttonGroup="buttonGroup" @click="onClick"
        @buttonClick="buttonClick" />
      <view class="btm_tool">
        <view class="tool_item">
          <!-- 客服 -->
          <view class="iconfont icon-kefu"></view>
          <view></view>
          <button open-type="contact"></button>
        </view>
        <view class="tool_item">
          <!-- 分享 -->
          <view class="iconfont icon-fenxiang"></view>
          <view></view>
          <button open-type="share"></button>
        </view>
      </view>

    </view>

  </view>
</template>

<script>
  // 从 vuex 中按需导出 mapState 辅助方法
  import {mapState,mapMutations,mapGetters} from 'vuex'

  export default {

    data() {
      return {
        // 用来代替无法显示轮播图时
        defaultPic: [

          {
            url: '../../static/image/1.webp'
          },
          {
            url: '../../static/image/2.webp'
          },
          {
            url: '../../static/image/3.webp'
          },
          {
            url: '../../static/image/4.webp'
          },
          {
            url: '../../static/image/5.webp'
          }
        ],

        // 商品详情对象
        goods_info: {},
        // 左侧按钮组的配置对象
        options: [{
          icon: 'headphones',
          text: '客服',
          // info:'2',
          // infoBackgroundColor:'#ffa200',
          // infoColor:'red'
        }, {
          icon: 'icon',
          text: '分享',
          // info:'2',

        }, {
          icon: 'cart',
          text: '购物车',
          // info: ''
        }],
        // 右侧按钮组的配置对象
        buttonGroup: [{
          text: '加入购物车',
          backgroundColor: 'linear-gradient(to right , #f9d423 , #ff870f)',
          color: '#fff'
        }, {
          text: '立即购买',
          backgroundColor: '#ff0004',
          color: '#fff'
        }]
      };
    },
    computed: {
      // 调用 mapState 方法，把 m_cart 模块中的 cart 数组映射到当前页面中，作为计算属性来使用
      // ...mapState('模块的名称', ['要映射的数据名称1', '要映射的数据名称2'])
      ...mapState('m_cart', ['cart']),
      ...mapGetters('m_cart',['total'])
    },
    watch:{
      total:{
        // 1. 监听 total 值的变化，通过第一个形参得到变化后的新值
        handler(newVal){
          const findResult = this.options.find((x)=>x.text === "购物车")
          if(findResult){
            // 3. 动态为购物车按钮的 info 属性赋值
            findResult.info = newVal
          }
        },
        // immediate 属性用来声明此侦听器，是否在页面初次加载完毕后立即调用
        immediate: true
      }
    },
    methods: {
      // 把 m_cart 模块中的 addToCart 方法映射到当前页面使用
      ...mapMutations('m_cart', ['addToCart']),
      async getGoodsDetail(goods_id) {
        const {
          data: res
        } = await uni.$http.get('/api/public/v1/goods/detail', {
          goods_id
        })
        if (res.meta.status !== 200) return uni.$showMsg()
        // console.log(res)
        // 使用字符串的 replace() 方法，为 img 标签添加行内的 style 样式，从而解决图片底部 空白间隙的问题     将 webp 的后缀名替换为 jpg 的后缀名 
        res.message.goods_introduce = res.message.goods_introduce.replace(/<img /g, '<img style="display:block;" ')
          .replace(/webp/g, 'jpg')
        this.goods_info = res.message

      },
      // 预览图片
      preview(index) {
        // 调用 uni.previewImage() 方法预览图片
        uni.previewImage({
          // 预览时，默认显示图片的索引  
          current: index,
          // 所有图片 url 地址的数组
          urls: this.goods_info.pics.map(x => x.pics_big)
        })
      },
      onClick(e) {
        // console.log(e)
        if (e.content.text === '购物车') {
          uni.switchTab({
            url: '/pages/cart/cart'
          })
        }
      },
      buttonClick(e) {
        if (e.content.text === '加入购物车') {
          // 2. 组织一个商品的信息对象
          const goods = {
            goods_id: this.goods_info.goods_id, // 商品的Id
            goods_name: this.goods_info.goods_name, // 商品的名称
            goods_price: this.goods_info.goods_price, // 商品的价格
            goods_count: 1, // 商品的数量
            goods_small_logo: this.goods_info.goods_small_logo, // 商品的图片
            goods_state: true // 商品的勾选状态
          }
          this.addToCart(goods)
          
        }
      }
    },
    onLoad(options) {
      // 获取商品 Id
      const goods_id = options.goods_id
      // 调用请求商品详情数据的方法
      this.getGoodsDetail(goods_id)
      // console.log(this.$data)
      // console.log(this.defaultPic)
    }
  }
</script>

<style lang="scss">


  .iconfont {
    font-family: "iconfont" !important;
    font-size: 16px;
    font-style: normal;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  .icon-fenxiang:before {
    content: "\e86e";
    position: absolute;
    top: 16rpx;
    left: 36rpx;
    font-weight: 600;
    color: #686868;

  }

  .goods-detail {
    padding-bottom: 50px;

    swiper {
      height: 750rpx;

      image {
        width: 100%;
        height: 100%;
      }

    }

    .goods-info-box {
      padding: 10px;
      padding-right: 0;

      .price {
        color: #c00000;
        font-size: 18px;
        margin: 10px 0;
      }

      .goods-info-body {
        display: flex;
        justify-content: space-between;

        .goods-name {
          font-size: 13px;
          padding-right: 10px;
        }

        .favi {
          width: 120px;
          font-size: 12px;
          display: flex;
          border-left: 1px solid #efefef;
          align-items: center;
          flex-direction: column;
          color: gray;
        }
      }

      .yf {
        margin: 10px 0;
        font-size: 12px;
        color: gray;
      }
    }



    .goods_nav {
      position: fixed;
      bottom: 0;
      left: 0;
      width: 100%;
    }

    .btm_tool {
      // border-top: 1rpx solid #ccc;
      position: fixed;
      left: 0;
      bottom: 0;
      width: 25%;
      height: 100rpx;
      // background-color: #c00000;
      display: flex;
      z-index: 999;

      // opacity: 10%;
      .tool_item {
        // background-color: #c00000;
        // opacity: 20%;
        flex: 1;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        font-size: 24rpx;
        position: relative;

        button {
          position: absolute;
          ;
          // margin: 0 4px;
          top: 0;
          left: 0px;
          width: 100%;
          height: 100%;
          opacity: 0;

        }

      }
    }




  }
</style>
