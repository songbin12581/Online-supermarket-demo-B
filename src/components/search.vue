<template>
  <a-form-model
    layout="inline"
    :model="searchForm"
    @submit="handleSubmit"
    @submit.native.prevent
    class="search-box"
  >
    <a-form-model-item label="检索关键字">
      <a-input
        v-model="searchForm.searchWord"
        placeholder="请输入关键字"
      ></a-input>
    </a-form-model-item>
    <a-form-model-item label="商品类目">
      <a-select
        show-search
        placeholder="请选择商品类目"
        style="width: 200px"
        @change="handleChange"
        allowClear
      >
        <a-select-option v-for="c in data" :key="c.id" :value="c.id">
          {{ c.name }}
        </a-select-option>
      </a-select>
    </a-form-model-item>
    <a-form-model-item>
      <a-button
        type="primary"
        html-type="submit"
      >搜索</a-button>
    </a-form-model-item>
  </a-form-model>
</template>

<script>

export default {
  data() {
    return {
      searchForm: {
        searchWord: '',
        category: '',
      },
    };
  },
  props: ['data'],
  methods: {
    // 提交表单触发
    handleSubmit() {
      this.$emit('submit', this.searchForm);// 子组件向父组件传递数据
    },
    handleChange(val) {
      this.searchForm.category = val;
    },
  },
  created() {

  },
};
</script>

<style scoped>
  .search-box {
    padding: 10px 40px;
  }
</style>
