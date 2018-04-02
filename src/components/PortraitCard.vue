<template>
    <li class="inline-block p-2 m-2 rounded shadow border border-grey-lightest bg-white">
        <button @click="action"><img :src="imgSrc" class="shadow rounded"/>
        <span class="block uppercase">{{name}}</span></button>
    </li>
</template>

<script>
export default {
  name: 'Card',
  props: {
    name: String,
    imgSrc: String,
    action: Function,
  },
};
</script>

<style scoped>
img {
    max-width: 200px;
    max-height: 200px;
}
</style>
