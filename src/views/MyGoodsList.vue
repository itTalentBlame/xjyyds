<template>
  <div>
    <MyTable :arr="list">
      <template #header>
        <th style="text-align: center">#</th>
        <th>商品名称</th>
        <th>价格</th>
        <th>标签</th>
        <th>操作</th>
      </template>

      <template #tbody="scope">
        <td style="text-align: center">{{ scope.row.id }}</td>
        <td>{{ scope.row.goods_name }}</td>
        <td style="text-align: center">{{ scope.row.goods_price }}</td>
        <td>
          <input
            type="text"
            class="tag-input form-control"
            style="width: 100px"
            v-focus
            v-model="scope.row.inputValue"
            v-if="scope.row.inputVisible"
            @blur="scope.row.inputVisible = false"
            @keydown.enter="enterFn(scope.row)"
            @keydown.esc="scope.row.inputValue = ''"
          />
          <button
            style="margin-right: 30px"
            class="btn btn-primary btn-sm add-ta"
            v-else
            @click="scope.row.inputVisible = true"
          >
            +Tag
          </button>
          <span
            style="margin-right: 8px"
            class="badge badge-info"
            v-for="(item, index) in scope.row.tags"
            :key="index"
            >{{ item }}</span
          >
        </td>
        <td>
          <button
            class="btn btn-danger btn-sm"
            @click="del(scope.row.id)"
            v-isAllFn="scope.row"
          >
            删除
          </button>
        </td>
      </template>
    </MyTable>
  </div>
</template>

<script>
import MyTable from '../components/MyTable.vue'
export default {
  components: {
    MyTable,
  },
  data() {
    return {
      list: [],
    }
  },
  created() {
    this.getList()
  },
  methods: {
    getList() {
      this.$axios({
        url: '/api/goods',
      }).then((res) => {
        res.data.data.forEach((ele) => {
          ele.isShow = Math.floor(Math.random() * 2)
        })
        this.list = res.data.data
      })
    },
    del(id) {
      const index = this.list.findIndex((ele) => ele.id == id)
      this.list.splice(index, 1)
    },
    enterFn(obj) {
      if (!obj.inputValue.trim()) return alert('请输入内容')
      obj.tags.push(obj.inputValue)
      obj.inputValue = ''
    },
  },
  directives: {
    isAllFn: {
      inserted(el, binding) {
        if (binding.value.isShow) {
          el.style.display = 'none'
        }
      },
    },
  },
}
</script>

<style></style>
