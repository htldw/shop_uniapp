<template>
  <view>
    <!-- 使用自定义搜索组件 -->
    <my-search @click="gotoSearch"></my-search>
    <view class="scroll-view-container">
      <!-- 左侧的滑动区域 -->
      <scroll-view class="left-scroll-view" scroll-y="true" :style="{height: wh + 'px'}">
        <block v-for="(item,index) in cateList" :key="index">
          <view :class="['left-scroll-view-item', index===active?'active':'']" @click="activeChanged(index)">{{item.cat_name}}</view>
        </block>
      </scroll-view>
      <!-- 右侧的滑动区域 -->
      <scroll-view class="right-scroll-view" scroll-y="true" :style="{height: wh + 'px'}" :scroll-top="scrollTop">
        <view class="cate-lv2" v-for="(item2,index2) in cateLevel2" :key="index2">
          <!-- 二级分类的标题 -->
          <view class="cate-lv2-title">/{{item2.cat_name}}/</view>
          <!-- 三级分类列表 -->
          <view class="cate-lv3-list">
            <view class="cate-lv3-item" v-for="(item3,index3) in item2.children" :key="index3" @click="gotoGoodsList(item3)">
              <image  :src="item3.cat_icon" mode=""></image>
              <text>{{item3.cat_name}}</text>
            </view>
          </view>
        </view>
       
      </scroll-view>
    </view>
  </view>
</template>

<script>
  export default {
    data() {
      return {
        // 滚动条距离顶部的距离
        scrollTop: 0,
        // 当前选中项的索引，默认让第一项被选中
        active: 0,
        // 窗口的可用高度 = 屏幕高度 - navigationBar高度 - tabBar 高度
        wh: 0,
        // 分类列表数据
        cateList: [],        cateLevel2:[]
      };
    },
    onLoad() {
      // 一级分类的数据
      this.getCateList();
      // 二级分类的数据
      this.getCateLevel2();
      // 获取当前系统的信息
      const sysInfo = uni.getSystemInfoSync()
      // 为 wh 窗口可用高度动态赋值
      this.wh = sysInfo.windowHeight -50;
    },
    methods:{
      // 获取 一级分类列表数据
      async getCateList(){
        const {data:res} = await uni.$http.get('/api/public/v1/categories')
        // console.log(res);
        if(res.meta.status!==200) return uni.$showMsg()
        this.cateList = res.message
      },
      // 获取二级分类列表的数据
      async getCateLevel2(){
        const {data:res} = await uni.$http.get('/api/public/v1/categories')
        if(res.meta.status!==200) return uni.$showMsg()
        // console.log(res);
        this.cateLevel2 = res.message[0].children
      },
      // 点击一级分类时
      activeChanged(index){
        this.active = index;
        this.cateLevel2 = this.cateList[index].children
        this.scrollTop = this.scrollTop === 0?1:0
      },
      // 三级分类跳转
      gotoGoodsList(item3){
        uni.navigateTo({
        url: '/subpkg/goods_list/goods_list?cid=' + item3.cat_id
        })
      },
      gotoSearch(){
        uni.navigateTo({
          url:'/subpkg/search/search'
        })
      }
    }
  }
</script>

<style lang="scss">
  .scroll-view-container{
    display: flex;
    .left-scroll-view{
      width: 120px;
      .left-scroll-view-item{
        background-color: #F7F7F7;
        line-height: 60px;
        text-align: center;
        font-size: 12px;
        // 激活项的样式
        &.active{
          background-color: #ffffff;
          position: relative;
          // 渲染激活项左侧的红色指示边线
          &::before {
          content: ' ';
          display: block;
          width: 3px;
          height: 30px;
          background-color: #c00000;
          position: absolute;
          left: 0;
          top: 50%;
          transform: translateY(-50%);
          }
        }
      }
    }
  
    .cate-lv2-title{
      font-size: 12px;
      font-weight: bold;
      text-align: center;
      padding: 15px 0;
    }
    .cate-lv3-list{
      display: flex;
      flex-wrap: wrap;
      .cate-lv3-item{
        width: 33.33%;
        display: flex;
        margin-bottom: 10px;
        flex-direction: column;
        align-items: center;
        image {
        width: 60px;
        height: 60px;
        }
        text {
          font-size: 12px;
        }
      }
    }
  }
</style>
