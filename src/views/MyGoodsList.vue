<template>
  <div>
    <MyTable :list="list">
      <template v-slot:thead>
        <th>#</th>
        <th>商品名称</th>
        <th>价格</th>
        <th>标签</th>
        <th>操作</th>
      </template>
      <template v-slot:tbody="scoped">
        <tr v-for="item in scoped.row" :key="item.id">
          <td>{{ item.id }}</td>
          <td>{{ item.goods_name }}</td>
          <td>{{ item.goods_price }}</td>
          <td>{{ item.tags }}</td>
          <td>
            <button class="btn btn-danger btn-sm" style="margin-right: 8px">
              删除
            </button>
            <button
              class="btn btn-danger btn-sm"
              style="background: skyblue; border: 1px solid skyblue"
            >
              编辑
            </button>
          </td>
        </tr>
      </template>
    </MyTable>
  </div>
</template>

<script>
import MyTable from "../components/MyTable.vue";
export default {
  data() {
    return {
      list: [],
    };
  },
  components: {
    MyTable,
  },
  created() {
    this.$axios({
      url: "api/goods",
    }).then((res) => {
      console.log(res.data.data);
      this.list = res.data.data;
    });
  },
};
</script>

<style></style>
