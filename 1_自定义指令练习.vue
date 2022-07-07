<template>
  <div>
    <input type="text" v-gfocus v-model="str" v-number="str" />{{ str }}
    <p v-color="colorStr" @click="colorStr = 'green'">Hello World</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      str: '',
      colorStr: 'red',
    }
  },
  directives: {
    number: {
      update(el, binding, Vnode) {
        el.focus()
        el.value = el.value.replace(/\D+/g, '')
        // this.str = el.val
        Vnode.context[binding.expression] = el.value
      },
    },
    color: {
      inserted(el, binding) {
        el.style.color = binding.value
      },
      update(el, binding) {
        el.style.color = binding.value
      },
    },
  },
}
</script>

<style></style>
