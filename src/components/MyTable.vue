<template>
  <table class="table table-bordered table-stripped">
    <thead>
      <tr>
        <slot name="header"></slot>
      </tr>
    </thead>
    <tbody>
      <tr v-for="obj in arr" :key="obj.id">
        <slot name="tbody" :row="obj"></slot>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  props: {
    arr: Array,
  },
}
</script>

<style></style>
