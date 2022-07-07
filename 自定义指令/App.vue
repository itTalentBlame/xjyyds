<template>
  <div>
    {{ str || '好的' }}
    <input type="text" v-model="str" v-Number="str" v-focus />
  </div>
</template>

<script>
export default {
  data() {
    return {
      str: '',
    }
  },
  directives: {
    Number: {
      inserted(el, binding, vnode) {
        el.value = el.value.replace(/[^\d.]/g, '')
        vnode.context[binding.expression] = el.value
      },
      update(el, binding, vnode) {
        el.value = el.value.replace(/[^\d.]/g, '')
        vnode.context[binding.expression] = el.value
      },
    },
  },
}
</script>

<style></style>
