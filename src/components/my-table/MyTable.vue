<template>
  <table class="table table-bordered table-striped">
    <!-- 表格的标题区域 -->
    <thead>
      <tr>
        <!-- 命名插槽 -->
        <slot name="header"></slot>
      </tr>
    </thead>
    <!-- 表格的主体区域 -->
    <tbody>
      <!-- 使用 v-for 指令，循环渲染表格的数据行 -->
      <tr v-for="(item, index) in data" :key="item.id">
        <!-- 为数据行的 td 预留的作用域插槽 -->
        <slot name="body" :row="item" :index="index"></slot>
      </tr>
    </tbody>
  </table>
</template>
<script>
export default {
  name: 'MyTable',
  props: {
    // 表格的数据源
    data: {
      type: Array,
      required: true,
      default: []
    }
  }
}
</script>
<style lang="less" scoped></style>
