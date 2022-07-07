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

      <template #tbody="scope">
        <td>{{ scope.row.id }}</td>
        <td>{{ scope.row.goods_name }}</td>
        <td>{{ scope.row.goods_price }}</td>
        <td>
          <input
            class="tag-input form-control"
            style="width: 100px"
            type="text"
            v-if="scope.row.inputVisible"
            v-gfocus
            v-model.trim="scope.row.inputValue"
            @blur="scope.row.inputVisible = false"
            @keyup.enter="enterFn(scope.row)"
            @keyup.esc="scope.row.inputValue = ''"
          />
          <button
            style="display: block"
            class="btn btn-primary btn-sm add-tag"
            v-else
            @click="scope.row.inputVisible = true"
          >
            +Tag
          </button>

          <span
            style="margin-right: 5px"
            class="badge badge-warning"
            v-for="(item, index) in scope.row.tags"
            :key="index"
            >{{ item }}</span
          >
        </td>
        <td>
          <button
            style="margin-right: 5px"
            class="btn btn-danger btn-sm"
            @click="del(scope.row.id)"
          >
            删除
          </button>
          <button
            type="button"
            class="btn btn-info btn-sm"
            v-isShow="scope.row.type"
          >
            编辑
          </button>
        </td>
      </template>
    </MyTable>
  </div>
</template>

<script>
import MyTable from '../components/MyTable.vue'
export default {
  data() {
    return {
      list: [],
    }
  },
  components: { MyTable },
  created() {
    this.getList()
  },
  methods: {
    getList() {
      this.$axios({
        url: '/api/goods',
      }).then((res) => {
        const newList = res.data.data
        newList.forEach((item) => {
          item.type = Math.floor(Math.random() * 10)
        })
        this.list = newList
      })
    },
    del(id) {
      const index = this.list.findIndex((item) => item.id === id)
      this.list.splice(index, 1)
    },
    enterFn(obj) {
      if (obj.inputValue == '') return alert('请输入内容')
      obj.tags.push(obj.inputValue)
      obj.inputValue = ''
    },
  },
  directives: {
    isShow: {
      inserted(el, binding) {
        const arr = [0, 1, 2] // 0 admin  1 test  2 dev
        if (arr.includes(binding.value)) {
          el.style.display = 'block'
        } else {
          el.style.display = 'none'
        }
      },
    },
  },
}
</script>

<style></style>
