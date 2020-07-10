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
  name: "LibraryList",
  data() {
    return {};
  },
  props: ["library"]
};
</script>

<style scoped>
#library-list,
#grid-container,
#grid-header {
  width: 80%;
}
#library-list {
  margin: auto;
}
#grid-header {
  margin: 100px auto 20px auto;
  display: grid;
  grid-template-areas: "head-author head-title head-pages head-read";
  grid-template-columns: 3fr 3fr 1fr 1fr;
}
#grid-container {
  margin: 5px auto 5px auto;
  display: grid;
  grid-template-areas: "author title pages read";
  grid-template-columns: 3fr 3fr 1fr 1fr;
  /*grid-auto-flow: row;*/
}
.head-author {
  grid-area: head-author;
}
.head-title {
  grid-area: head-title;
}
.head-pages {
  grid-area: head-pages;
}
.head-read {
  grid-area: head-read;
}
.author {
  grid-area: author;
}
.title {
  grid-area: title;
}
.pages {
  grid-area: pages;
}
.book-read {
  grid-area: read;
}
</style>
