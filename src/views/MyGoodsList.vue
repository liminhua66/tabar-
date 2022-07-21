<template>
  <div>
    <MyTable :goodsList="goodsList">
      <template #header>
        <th>#</th>
        <th>商品名称</th>
        <th>价格</th>
        <th>标签</th>
        <th>操作</th>
      </template>
      <template #content="scope">
        <td>{{ scope.row.id }}</td>
        <td>{{ scope.row.goods_name }}</td>
        <td>{{ scope.row.goods_price }}</td>
        <td>
          <input
            type="text"
            class="tag-input form-control"
            style="width: 100px"
            v-if="scope.row.inputVisible"
            @keyup.esc="scope.row.inputVisible = false"
            @keyup.enter="add(scope.row)"
            @blur="scope.row.inputVisible = false"
            v-model="scope.row.inputValue"
            v-focus
          />
          <button
            class="btn btn-primary btn-sm add-tag"
            v-else
            @click="scope.row.inputVisible = true"
          >
            +Tag
          </button>
          <span
            style="margin-right: 8px"
            class="badge badge-warning"
            v-for="(a, ind) in scope.row.tags"
            :key="ind"
            >{{ a }}</span
          >
        </td>
        <td>
          <button class="btn btn-danger btn-sm" @click="del(scope.row.id)">
            删除
          </button>
        </td>
      </template>
    </MyTable>
  </div>
</template>

<script>
import MyTable from "../components/MyTable.vue";
export default {
  name: "MyGoodsList",
  components: { MyTable },
  data() {
    return {
      goodsList: [],
    };
  },
  methods: {
    getGoods() {
      this.$axios({
        url: "/api/goods",
      }).then((res) => {
        this.goodsList = res.data.data;
      });
    },
    del(id) {
      const index = this.goodsList.findIndex((ele) => (ele.id = id));
      this.goodsList.splice(index, 1);
    },
    add(item) {
      if (item.inputValue.trim() == "") {
        alert("输入不能为空！");
        item.inputValue = "";
        return;
      } else {
        item.tags.push(item.inputValue);
        item.inputValue = "";
      }
    },
  },
  mounted() {
    this.getGoods();
  },
};
</script>

<style></style>
