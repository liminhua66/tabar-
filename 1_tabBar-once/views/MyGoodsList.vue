<template>
  <div>
    <MyTable :list="list">
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
            @blur="scope.row.inputVisible = false"
            @keyup.esc="scope.row.inputVisible = false"
            @keyup.enter="add(scope.row)"
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
            class="badge badge-warning"
            style="margin-right: 8px"
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
  data() {
    return {
      list: [],
      // inputValue: "",
    };
  },
  components: { MyTable },
  methods: {
    getGoods() {
      this.$axios({
        url: "/api/goods",
      }).then((res) => {
        this.list = res.data.data;
      });
    },
    del(id) {
      const index = this.list.findIndex((ele) => ele.id == id);
      this.list.splice(index, 1);
    },
    add(val) {
      if (val.inputValue.trim() == "") {
        alert("输入的值不能为空");
        val.inputValue = "";
        return;
      } else {
        val.tags.push(val.inputValue);
        val.inputValue = "";
      }
    },
  },
  mounted() {
    this.getGoods();
  },
};
</script>
