<template>
  <view class="my-userinfo-container">
    <!-- 顶部区域 -->
    <view class="top-box">
      <image class="avatar" :src="userinfo.avatarUrl" mode=""></image>
      <view class="nickname">{{userinfo.nickName}}</view>
    </view>
    <!-- 面板的列表区域 -->
    <view class="panel-list">
      <!-- 第一个面板 -->
      <view class="panel">
        <!-- panel 的主体区域 -->
        <view class="panel-body">
          <!-- panel 的 item 项 -->
          <view class="panel-item">
            <text>8</text>
            <text>收藏的店铺</text>
          </view>
          <view class="panel-item">
            <text>14</text>
            <text>收藏的商品</text>
          </view>
          <view class="panel-item">
            <text>18</text>
            <text>关注的商品</text>
          </view>
          <view class="panel-item">
            <text>84</text>
            <text>足迹</text>
          </view>
        </view>
      </view>

      <!-- 第二个面板 -->
      <view class="panel"  @click="goOrder">
        <!-- 标题 -->
        <view class="panel-title">我的订单</view>
        <!-- 主体 -->
        <view class="panel-body">
          <!-- item项 -->
          <view class="panel-item">
            <image src="/static/my-icons/icon1.png" class="icon"></image>
            <text>待付款</text>
          </view>
          <view class="panel-item">
            <image src="/static/my-icons/icon2.png" class="icon"></image>
            <text>待收货</text>
          </view>
          <view class="panel-item">
            <image src="/static/my-icons/icon3.png" class="icon"></image>
            <text>退款/退货</text>
          </view>
          <view class="panel-item" >
            <image src="/static/my-icons/icon4.png" class="icon"></image>
            <text>全部订单</text>
          </view>
        </view>
      </view>
      <!-- 第三个面板 -->
      <view class="panel">
        <view class="panel-list-item">
          <text>收货地址</text>
          <uni-icons type="arrowright" size="15"></uni-icons>
        </view>
        <view class="panel-list-item">
          <text>联系客服</text>
          <uni-icons type="arrowright" size="15"></uni-icons>
        </view>
        <view class="panel-list-item" @click="logout">
          <text>退出登录</text>
          <uni-icons type="arrowright" size="15"></uni-icons>
        </view>
      </view>

    </view>
  </view>
</template>
<script>
  // 按需导入 mapState 辅助函数
  import {
    mapState,
    mapMutations
  } from 'vuex'
  export default {
    computed: {
      // 将 m_user 模块中的 userinfo 映射到当前页面中使用
      ...mapState('m_user', ['userinfo']),
    },
    data() {
      return {}
    },
    methods: {
      ...mapMutations('m_user', ['updateUserInfo', 'updateToken', 'updateAddress']),
      async logout() {
        // 询问是否退出登录
        const [err, succ] = await uni.showModal({
          title: '提示',
          content: "确认退出登录吗？"
        }).catch(err => err)
        // console.log(succ);
        // 用户点击退出登录确定按钮
        if (succ && succ.confirm) {
          // 清空vuex里面的userinfo/token/address
          this.updateUserInfo({})
          this.updateToken('')
          this.updateAddress({})
        }
      },
      goOrder(){
        uni.navigateTo({
          url:'/subpkg/goods_orders/goods_orders'
        })
      }
    }
  }
</script>
<style lang="scss">
  .my-userinfo-container {
    height: 100%;
    background-color: #f4f4f4;

    // <!-- 顶部区域 -->
    .top-box {
      height: 400rpx;
      background-color: #c00000;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;

      .avatar {
        width: 90px;
        height: 90px;
        border-radius: 45px;
        border: 2px solid white;
        box-shadow: 0px 1px 5px black;
      }

      .nickname {
        color: white;
        font-weight: bold;
        font-size: 16px;
        margin-top: 10px;
      }

    }

    // 面板的列表区域
    .panel-list {
      padding: 0 10px;
      position: relative;
      top: -10px;

      // 每个面板
      .panel {
        background-color: white;
        border-radius: 3px;
        margin-bottom: 8px;

        // 第二个面板标题
        .panel-title {
          line-height: 45px;
          padding-left: 10px;
          font-size: 15px;
          border-bottom: 1px solid #f4f4f4;
        }

        //  panel 的主体区域
        .panel-body {
          display: flex;
          justify-content: space-around;

          // panel 的 item 项
          .panel-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            // justify-content: space-around;
            font-size: 13px;
            padding: 10px 0;
          }

          .icon {
            width: 35px;
            height: 35px;
          }
        }

        .panel-list-item {
          height: 45px;
          display: flex;
          justify-content: space-between;
          align-items: center;
          font-size: 15px;
          padding: 0 10px;
        }
      }
    }
  }
</style>
