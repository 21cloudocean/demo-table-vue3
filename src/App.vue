<template>
  <div>
    <h1>App 根组件</h1>
    <!-- 使用表格组件 -->
    <my-table :data="goodslist">
      <template #header>
        <th>#</th>
        <th>商品名称</th>
        <th>价格</th>
        <th>标签</th>
        <th>操作</th>
      </template>

      <template #body="{ row, index }">
        <td>{{ index + 1 }}</td>
        <td>{{ row.goods_name }}</td>
        <td>￥{{ row.goods_price }}</td>
        <td>{{ row.tags }}</td>
        <td>
          <button
            type="button"
            class="btn btn-danger btn-sm"
            @click="onRemove(row.id)"
          >
            删除
          </button>
        </td>
      </template>
    </my-table>
  </div>
</template>
<script>
import MyTable from './components/my-table/MyTable.vue'

export default {
  components: { MyTable },
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
    },
    // 根据 Id 删除商品信息
    onRemove(id) {
      this.goodslist = this.goodslist.filter((x) => x.id !== id)
    }
  },
  created() {
    this.getGoodsList()
  }
}
</script>
<style lang="less" scoped></style>
