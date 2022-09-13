<template>
  <view>
    <view class="search-box">
      <!-- 使用 uni-ui 提供的搜索组件 -->
      <uni-search-bar @input="input" :radius="100" cancelButton="none"></uni-search-bar>
    </view>
    <!-- 搜索建议列表 -->
    <view class="sugg-list" v-if="searchResults.length !== 0">
      <view class="sugg-item" v-for="(item,index) in searchResults" :key="index" @click="gotoDetail(item.goods_id)">
        <view class="goods-name">{{item.goods_name}}</view>
        <uni-icons type="arrowright" size="16"></uni-icons>
      </view>
    </view>
    <!-- 搜索历史 -->
    <view class="history-box" v-else>
      <!-- 标题区域 -->
      <view class="history-title">
        <text>搜索历史</text>
        <uni-icons type="trash" size="17" @click="cleanHistory"></uni-icons>
      </view>
      <!-- 列表区域 -->
      <view class="history-list">
        <uni-tag :text="item" v-for="(item,index) in historys" :key="index" @click="gotoGoodsList(item)"></uni-tag>
      </view>
    </view>
  </view>
</template>

<script>
  export default {
    data() {
      return {
        // 延时器的 timerId
        timer: null,
        // 搜索关键词
        kw: "",
        // 搜索列表
        searchResults: [],
        // 搜索关键词的历史记录
        historyList: ['a', 'app', 'apple']
      };
    },
    methods: {
      input(e) {
        // 清除 timer 对应的延时器
        clearTimeout(this.timer)
        // 重新启动一个延时器，并把 timerId 赋值给 this.timer
        this.timer = setTimeout(() => {
          // 如果 500 毫秒内，没有触发新的输入事件，则为搜索关键词赋值
          this.kw = e.trim()
          // console.log(this.kw)
          // 根据关键词，查询搜索建议列表
          this.getSearchResults()
        }, 500)
      },
      // 根据搜索关键词，搜索商品建议列表
      async getSearchResults() {
        // 如果搜索内容为空
        if (this.kw.length === 0) {
          this.searchResults = []
          return
        }
        // 发起请求，获取搜索建议列表
        const {
          data: res
        } = await uni.$http.get('/api/public/v1/goods/qsearch', {
          query: this.kw
        })
        if (res.meta.status !== 200) return uni.$showMsg()
        this.searchResults = res.message
        this.SaveSearchHistory()
      },
      // 保存搜索关键词为历史记录
      SaveSearchHistory() {
        // this.historyList.push(this.kw)
        // 1. 将 Array 数组转化为 Set 对象
              const set = new Set(this.historyList)
        // 2. 调用 Set 对象的 delete 方法，移除对应的元素
              set.delete(this.kw)
        // 3. 调用 Set 对象的 add 方法，向 Set 中添加元素
              set.add(this.kw)
        // 4. 将 Set 对象转化为 Array 数组
              this.historyList = Array.from(set)
        // 调用 uni.setStorageSync(key, value) 将搜索历史记录持久化存储到本地
              uni.setStorageSync('kw',JSON.stringify(this.historyList))
      },
      // 搜索列表点击跳转到商品详情
      gotoDetail(goods_id) {
        uni.navigateTo({
          url: '/subpkg/goods_detail/goods_detail?goods_id=' + goods_id
        })
      },
      // 搜索历史列表跳转
      gotoGoodsList(kw){
        uni.navigateTo({
          url: '/subpkg/goods_list/goods_list?query=' + kw
        })
      },
      // 清空历史记录
      cleanHistory(){
        // 清空 data 中保存的搜索历史
        this.historyList=[]
        // 清空本地存储中记录的搜索历史
        uni.setStorageSync('kw','[]')
      }
    },
    computed: {
      historys() {
        return [...this.historyList].reverse()
      }
    },
    onLoad() {
      this.historyList = JSON.parse(uni.getStorageSync('kw') || '[]')
    }
  }
</script>

<style lang="scss">
  .search-box {
    // 设置定位效果为“吸顶”
    position: sticky;
    // 吸顶的“位置”
    top: 0;
    // 提高层级，防止被轮播图覆盖
    z-index: 999;
  }

  .sugg-list {
    padding: 0 5px;

    .sugg-item {
      font-size: 12px;
      padding: 13px 0;
      border-bottom: 1px solid #efefef;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    .goods-name {
      // 文字不允许换行（单行文本）
      white-space: nowrap;
      // 溢出部分隐藏
      overflow: hidden;
      // 文本溢出后，使用 ... 代替
      text-overflow: ellipsis;
      margin-right: 3px;
    }
  }

  .history-box {
    padding: 0 5px;

    .history-title {
      display: flex;
      justify-content: space-between;
      align-items: center;
      height: 40px;
      font-size: 13px;
      border-bottom: 1px solid #efefef;
    }

    .history-list {
      display: flex;
      flex-wrap: wrap;

      .uni-tag {
        display: block;
        margin-top: 5px;
        margin-right: 5px;
        background-color: #fafafa;
        color: #4a4a4a;
        border: none;
        font-weight: 400;
      }
    }
  }
</style>
