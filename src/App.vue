<template>
  <div>
    <h1>App 根组件</h1>
  </div>
</template>
<script>
export default {
  name: 'MyApp',
  data() {
    return {
      // 商品列表数据
      goodslist: []
    }
  },
  methods: {
    // 初始化商品列表的数据
    async getGoodsList() {
      // 发起 Ajax 请求
      const { data: res } = await this.$http.get('/api/goods')
      // 请求失败
      if (res.status !== 0) {
        return console.log('获取商品列表失败')
      }
      // 请求成功
      this.goodslist = res.data
      console.log(res.data)
    }
  },
  created() {
    this.getGoodsList()
  }
}
</script>
<style lang="less" scoped></style>
