<template>
  <!-- 商品列表页面 -->
  <div class="product-list">
    <!-- 搜索页面 -->
    <search-box @submit="searchSubmit" :data="categoryList" />
    <a-button class="product-add-btn">
      <router-link :to="{ name: 'ProductAdd' }">添加商品</router-link>
    </a-button>
    <!-- 表格页面 -->
    <product-table
      :data="tableData"
      :page="page"
      @change="changePage"
      :categoryList="categoryList"
      @edit="editProduct"
      @remove="removeProduct"
    />
  </div>
</template>

<script>
import searchBox from '../../components/search.vue';
import productTable from '../../components/productTable.vue';
import api from '../../api/product';
import categoryApi from '../../api/category';

export default {
  data() {
    return {
      tableData: [],
      searchForm: [],
      categoryList: [],
      page: {
        current: 1,
        pageSize: 10,
        showSizeChanger: true,
        total: 1,
      },
      categoryObj: {},
    };
  },
  components: {
    searchBox,
    productTable,
  },
  async created() {
    await categoryApi.list().then((res) => {
      this.categoryList = res.data;
      res.data.forEach((item) => {
        this.categoryObj[item.id] = item;
      });
    });
    this.getTableData();
  },
  methods: {
    searchSubmit(form) {
      this.searchForm = form;
      // 用来触发搜索功能，点击重新刷新页面
      this.getTableData();
    },
    getTableData() {
      api
        .list({
          page: this.page.current,
          size: this.page.pageSize,
          ...this.searchForm,
        })
        .then((res) => {
          this.page.total = res.total;
          this.tableData = res.data.map((item) => ({
            ...item,
            categoryName: this.categoryObj[item.category].name,
          }));
        });
    },
    changePage(page) {
      this.page = page;
      this.getTableData();
    },
    editProduct(record) {
      this.$router.push({
        name: 'ProductEdit',
        params: {
          id: record.id,
        },
      });
    },
    removeProduct(record) {
      this.$confirm({
        title: '确认删除',
        content: () => (
          <div style="color:red;">{`您真的要删除:${record.title}吗`}</div>
        ),
        onOk: () => {
          api
            .remove({
              id: record.id,
            })
            .then(() => {
              this.getTableData();
            });
        },
        onCancel() {
        },
        class: 'confirm-box',
      });
    },
  },
};
</script>

<style scoped>
.product-list {
  position: relative;
}
.product-add-btn {
  position: absolute;
  right: 10px;
  top: 14px;
}
</style>
