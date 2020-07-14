<template>
  <div id="library-list">
    <div id="grid-header">
      <span class="head-author">Author</span>
      <span class="head-title">Title</span>
      <span class="head-pages">No. Pages</span>
      <span class="head-read">Have Read?</span>
    </div>
    <div>
      <span id="grid-container" v-for="book in library" :key="book.title">
        <span class="author">{{ book.author }}</span>
        <span class="title">{{ book.title }}</span>
        <span class="pages">{{ book.pages }}</span>
        <span class="book-read">{{ book.read }}</span>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: "LibraryData",
  props: {
    library: Array
  }
};
</script>

<style></style>
