<template>
  <button
    class="border border-red text-red rounded-full px-4 py-2 uppercase mx-2 mb-2 shadow"
    @click="click">
    {{title}}
  </button>
</template>

<script>
export default {
  props: {
    click: Function,
    title: String,
  },
};
</script>

<style scoped>
button {
  background: rgba(255, 255, 255, 0.6);
}
</style>

