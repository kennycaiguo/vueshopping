<template>
  <div>
    <a :href="item.link" class="image">
      <img :src="item.image" alt="">
      <span class="title">{{item.title}}</span>
    </a>
  </div>
</template>

<script>
  export default {
    name: "subcategory",
    props: {
      item: {
        type: Object,
        default() {
          return {}
        }
      }
    }
  }
</script>

<style scoped>
img {
  display: block;
}
</style>
