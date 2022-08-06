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
        <!-- 循环渲染标签信息 -->
        <!-- 因为标签不重复，所以把标签做key值 -->
        <td>
          <!-- 基于当前行的 inputVisible，来控制 input 和 button 的按需展
示-->
          <!-- 使用v-model和绑定blur事件，让文本框失去焦点后自动隐藏 -->
          <!-- 绑定keyup事件，按下enter件即触发保存tag的method -->
          <!-- 按下esc清空输入框内容 -->
          <input
            type="text"
            class="form-control form-control-sm ipt-tag"
            v-if="row.inputVisible"
            v-focus
            v-model.trim="row.inputValue"
            @blur="onInputConfirm(row)"
            @keyup.enter="onInputConfirm(row)"
            @keyup.esc="row.inputValue = ''"
          />
          <button
            type="button"
            class="btn btn-primary btn-sm"
            v-else
            @click="row.inputVisible = true"
          >
            +Tag
          </button>
          <span
            class="badge badge-warning ml-2"
            v-for="item in row.tags"
            :key="item"
            >{{ item }}</span
          >
        </td>
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
    },
    onInputConfirm(row) {
      // 1. 把用户在文本框中输入的值，预先转存到常量 val 中
      const val = row.inputValue
      // 2. 清空文本框的值
      row.inputValue = ''
      // 3. 隐藏文本框
      row.inputVisible = false

      // 为商品添加新的 tag 标签

      // 1.1 判断 val 的值是否为空，如果为空，则不进行添加
      // 1.2 判断 val 的值是否已存在于 tags 数组中，防止重复添加
      if (!val || row.tags.indexOf(val) !== -1) return
      // 2. 将用户输入的内容，作为新标签 push 到当前行的 tags 数组中
      row.tags.push(val)
    }
  },
  // 让文本框自动获得焦点
  directives: {
    focus(el) {
      el.focus()
    }
  },
  created() {
    this.getGoodsList()
  }
}
</script>
<style lang="less" scoped></style>
