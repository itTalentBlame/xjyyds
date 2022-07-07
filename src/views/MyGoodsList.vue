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
          <td>
            <input
              type="text"
              class="tag-input form-control"
              style="width: 100px"
              v-if="item.inputVisible"
              v-focus
              @blur="item.inputVisible = !item.inputVisible"
              v-model="item.inputValue"
              @keyup.enter="enterFn(item)"
              @keyup.esc="item.inputValue = ''"
            />
            <button
              class="btn btn-primary btn-sm add-tag"
              v-else
              @click="item.inputVisible = !item.inputVisible"
            >
              +Tag
            </button>
            <span
              class="badge badge-info"
              v-for="(obj, index) in item.tags"
              :key="index"
              style="margin-right: 8px"
              >{{ obj }}</span
            >
          </td>
          <td>
            <button
              class="btn btn-danger btn-sm"
              style="margin-right: 8px"
              v-isShow="item.isShow"
              @click="del(item.id)"
            >
              删除
            </button>
            <button
              class="btn btn-danger btn-sm"
              style="background: skyblue; border: 1px solid skyblue"
              v-isShow="item.isShow"
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
      isShow: [0, 1, 0, 0, 0, 1, 1, 1, 0, 1],
    };
  },
  components: {
    MyTable,
  },
  created() {
    this.$axios({
      url: "api/goods",
    }).then((res) => {
      res.data.data.forEach((item, index) => {
        item.isShow = this.isShow[index];
      });
      this.list = res.data.data;
      console.log(this.list);
    });
  },
  directives: {
    isShow: {
      inserted(el, binding) {
        if (binding.value) {
          el.style.display = "none";
        }
      },
    },
    focus: {
      inserted(el) {
        el.focus();
      },
    },
  },
  methods: {
    del(id) {
      const index = this.list.findIndex((item) => item.id == id);
      this.list.splice(index, 1);
    },
    enterFn(item) {
      if (!item.inputValue) return;
      item.tags.push(item.inputValue);
      item.inputValue = "";
    },
  },
};
</script>

<style></style>
