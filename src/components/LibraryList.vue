<template>
  <div id="library-list">
    <div v-for="book in library" :key="book.title">
      {{ book.author }} - {{ book.title }} - {{ book.pages }} - {{ book.read }}
    </div>
  </div>
</template>

<script>
export default {
  name: "LibraryList",
  data() {
    return {};
  },
  props: ["library"],
};
</script>

<style scoped>
#library-list {
  margin-top: 100px;
}
</style>
