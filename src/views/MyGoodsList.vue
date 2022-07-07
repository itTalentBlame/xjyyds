<template>
  <div>
    <MyTable :arr="list">
      <template #header>
        <tr>
          <th>#</th>
          <th>商品名称</th>
          <th>价格</th>
          <th>标签</th>
          <th>操作</th>
        </tr>
      </template>
      <template #bodys="scope">
        <td>{{ scope.row.id }}</td>
        <td>{{ scope.row.goods_name }}</td>
        <td>{{ scope.row.goods_price }}</td>
        <td>
          <input
            type="text"
            class="tag-input form-control"
            style="width: 100px"
            v-if="scope.row.inputShow"
            @keydown.enter="addFn(scope.row)"
            @keydown.esc="scope.row.inputValue = ''"
            @blur="scope.row.inputShow = false"
            v-model="scope.row.inputValue"
          />
          <button
            class="btn btn-primary btn-sm add-tag"
            v-else
            @click="scope.row.inputShow = true"
          >
            +Tag
          </button>
          <span
            class="badge badge-warning"
            v-for="item in scope.row.tags"
            :key="item.id"
            >{{ item }}</span
          >
        </td>

        <td>
          <button
            class="btn btn-danger btn-sm"
            @click="del(scope.row.id)"
            style="display: block"
          >
            删除
          </button>
        </td>
      </template>
    </MyTable>
  </div>
</template>

<script>
import MyTable from '../components/MyTable.vue';
export default {
  components: { MyTable },
  data() {
    return {
      list: [],
    };
  },
  created() {
    this.$axios({
      url: '/api/goods',
    }).then((res) => {
      console.log(res);
      res.data.data.forEach((ele) => {
        ele.inputShow = false;
      });
      this.list = res.data.data;
    });
  },
  methods: {
    del(id) {
      const index = this.list.findIndex((ele) => (ele.id = id));
      this.list.splice(index, 1);
    },
    addFn(val) {
      if (val.inputValue.trim() == '') {
        return alert('请输入内容');
      }
      val.tags.push(val.inputValue);
      val.inputValue = '';
    },
  },
};
</script>

<style></style>
