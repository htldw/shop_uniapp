<template>

  <view class="goods-list">
    <view v-for="(item,index) in goodsList" :key="index">
      <my-goods :goods="item"></my-goods>
    </view>
  </view>


</template>

<script>
  export default {
    data() {
      return {
        // 商品列表数据
        goodsList: [],
        // 总条数
        total: '',
        queryObj: {
          // 搜索历史参数
          query: '',
          // 三级分类 参数
          cid: '',
          // 当前页码数
          pagenum: 1,
          // 每页显示多少条数据
          pageSize: 10,
        },
        // 节流阀
        isLoding: false
      }
    },
    methods: {
      async getGoodsList(cb) {

        // console.log(this);
        this.isLoding = true
        const {
          data: res
        } = await uni.$http.get('/api/public/v1/goods/search', this.queryObj)
        this.isLoding = false
        // 只要数据请求完毕，就立即按需调用 cb 回调函数
        cb && cb()
        // console.log(cb);

        if (res.meta.status !== 200) return uni.$showMsg();
        // 为数据赋值：通过展开运算符的形式，进行新旧数据的拼接

        this.goodsList = [
          ...this.goodsList,
          ...res.message.goods
        ]
        this.total = res.message.total
        // console.log(this.goods_list);
      },
      
    },
    onLoad(options) {
      // console.log(options);

      this.queryObj.query = options.query || '';
      this.queryObj.cid = options.cid || '';
      this.getGoodsList()
    },
    // 触底的事件
    onReachBottom() {
      if (this.queryObj.pagenum * this.queryObj.pageSize >= this.total)
        return uni.$showMsg('没有下一页了！')
      if (this.isLoding) return
      // 页面自增加一
      this.queryObj.pagenum += 1
      this.getGoodsList()
    },
    onPullDownRefresh() {
      // 重置关键数据
      this.queryObj.pagenum = 1
      this.total = 0
      this.isLoding = false
      this.goodsList = []
      // 重新发起请求
      this.getGoodsList(() => uni.stopPullDownRefresh())
    }
  }
</script>

<style lang="scss">

</style>
