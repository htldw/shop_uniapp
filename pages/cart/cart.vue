<template>
  <view>
    cart
  </view>
</template>

<script>
  export default {
    data() {
      return {
        cateList: []
      };
    },
    onLoad() {
      this.getCateList()
    },
    methods:{
      async getCateList(){
        const {data:res} = await uni.$http.get('/api/public/v1/categories');
        console.log(res);
        if(res.meta.status!==200) return uni.$showMsg()
        this.cateList = res.message
      }
    }
  }
</script>

<style lang="scss">

</style>
