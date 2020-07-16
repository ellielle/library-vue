<template>
  <div id="library-list">
    <div id="grid-header">
      <div class="head-author">Author</div>
      <div class="head-title">Title</div>
      <div class="head-pages">No. Pages</div>
      <div class="head-read">Have Read?</div>
      <div class="head-edit"></div>
      <div class="head-delete"></div>
    </div>
    <div id="grid-container" v-for="book in library" :key="book.title">
      <div class="author">{{ book.author }}</div>
      <div class="title">{{ book.title }}</div>
      <div class="pages">{{ book.pages }}</div>
      <input
        class="book-read"
        type="checkbox"
        v-model="book.read"
        @click="toggleRead(book.title)"
      />
      <div class="edit-book">
        <button class="btn-edit" @click="editBook(book.title)"></button>
      </div>
      <div class="delete-book">
        <button class="btn-delete" @click="removeFromLibrary(book.title)">
          X
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { eventBus } from "../main";

export default {
  name: "LibraryData",
  props: {
    library: Array
  },
  methods: {
    removeFromLibrary(bookTitle) {
      eventBus.$emit("removeBookFromLibrary", bookTitle);
    },
    editBook(bookTitle) {
      // TODO emit to formVisible
      // TODO look at refactoring new-data template to work with new and edit
      eventBus.$emit("formVisible", bookTitle);
    },
    toggleRead(bookTitle) {
      let index = this.library.findIndex(
        removedBook => removedBook.title === bookTitle
      );
      this.library[index].read = !this.library[index].read;
      eventBus.$emit("updateBook", bookTitle, this.library[index])
    }
  }
};
</script>

<style lang="scss" scoped>
#library-list,
#grid-header,
#grid-container {
  width: 80%;
}
#library-list {
  margin: auto;
}
#grid-header {
  margin: 100px auto 20px auto;
  display: grid;
  grid-template-areas: "head-author head-title head-pages head-read head-edit .";
  grid-template-columns: 3fr 3fr 1fr 1fr 0.5fr 0.5fr;
}
#grid-container {
  margin: 5px auto 5px auto;
  display: grid;
  grid-template-areas: "author title pages read";
  grid-template-columns: 3fr 3fr 1fr 1fr 0.5fr 0.5fr;
  place-items: center;
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

.btn-delete,
.btn-edit {
  display: inline-block;
  border: none;
  padding: 15px 15px;
  margin: 0;
  text-decoration: none;

  font-family: sans-serif;
  font-size: 1rem;
  cursor: pointer;
  text-align: center;
  transition: background 250ms ease-in-out, transform 150ms ease;
}
.btn-delete {
  background: #0069ed;
  color: #ffffff;
  &:hover {
    background-color: red;
  }
}
.btn-edit {
  background: green;
  color: black;
  &:hover {
    background-color: purple;
  }
}
</style>
