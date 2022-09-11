<template>
  <view>
    <!-- 轮播图 -->
    <swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" circular>
      <swiper-item v-for="(item,index) in swiperList" :key="index">
        <navigator class="swiper-item" :url="'/subpkg/goods_detail/goods_detail?goods_id=' + item.goods_id">
          <image :src="item.image_src" mode=""></image>
        </navigator>
      </swiper-item>
    </swiper>
    <!-- 导航栏 -->
    <view class="nav-list">
      <view class="nav-item" v-for="(item,index) in navList" :key="index" @click="navClickHandler(item)">
        <image class="nav-img" :src="item.image_src" mode=""></image>
      </view>
    </view>
    <!-- 楼层 -->
    <view class="floor-list">
      <view class="floor-item" v-for="(item,index) in floorList" :key="index">
        <!-- 标题 -->
        <image class="floor-title" :src="item.floor_title.image_src" mode="">
        </image>
        <view class="floor-img-box">
          <!-- 左边图片 -->
          <navigator class="left-img-box" :url="item.product_list[0].url">
            <image :src="item.product_list[0].image_src" :style="{width:item.product_list[0].image_width + 'rpx'}"
              mode="widthFix"></image>
          </navigator>
          <!-- 右边图片 -->
          <view class="right-img-box">
            <navigator class="right-img-item" :url="item2.url" v-for="(item2,index2) in item.product_list" :key="index2" v-if="index2 !== 0">
              <image class="floor-right-img" :src="item2.image_src" mode="widthFix"
                :style="{width:item2.image_width + 'rpx'}"></image>
            </navigator>
          </view>

        </view>
      </view>

    </view>
  </view>

</template>

<script>
  export default {
    data() {
      return {
        // 轮播图数据
        swiperList: [],
        navList: [],
        floorList: []
      };
    },
    onLoad() {
      this.getSwiperList()
      this.getNavList()
      this.getFloorList()
    },
    methods: {
      // 3. 获取轮播图数据的方法
      async getSwiperList() {
        // 3.1 发起请求
        const {
          data: res
        } = await uni.$http.get('/api/public/v1/home/swiperdata');
        // console.log(res);
        // 3.2 请求失败
        if (res.meta.status !== 200) return uni.$showMsg()
        this.swiperList = res.message

        // 3.3 请求成功，为 data 中的数据赋值
      },
      async getNavList() {
        const {
          data: res
        } = await uni.$http.get('/api/public/v1/home/catitems');
        if (res.meta.status !== 200) return uni.$showMsg()
        this.navList = res.message

      },
      async getFloorList() {
        const {data: res} = await uni.$http.get('/api/public/v1/home/floordata');
        if (res.meta.status !== 200) return uni.$showMsg()
        res.message.forEach(floor => {
          floor.product_list.forEach(prod => {
            prod.url = '/subpkg/goods_list/goods_list?' + prod.navigator_url.split('?')[1]
          })
        })
        this.floorList = res.message
        // console.log(res);

      },
      navClickHandler(item) {
        if (item.name == '分类') {
          uni.switchTab({
            url: '/pages/cart/cart'
          })
        }
      }
    }
  }
</script>

<style lang="scss">
  // 轮播图
  swiper {
    height: 330rpx;

    .swiper-item,
    image {
      width: 100%;
      height: 100%;
    }
  }

  // 导航栏
  .nav-list {
    display: flex;
    justify-content: space-around;
    margin: 15px 0;

    .nav-img {
      width: 128rpx;
      height: 140rpx
    }
  }

  // 楼层
  .floor-list {
    .floor-title {
      height: 60rpx;
      width: 100%;
      display: flex;
    }

    .floor-img-box {
      display: flex;
      padding-left: 10rpx;

      .right-img-box {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-around;

        .right-img-item {}
      }
    }

  }
</style>
