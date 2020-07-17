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
        <button class="btn-edit" @click="editBook(book.title)">
          <svg viewBox="314.826 209.106 15 15" width="25" height="20">
            <path
              d="M 325.31 211.541 L 327.659 214.184 C 327.758 214.296 327.758 214.477 327.659 214.589 L 321.971 220.989 L 319.555 221.291 C 319.232 221.331 318.959 221.024 318.994 220.661 L 319.264 217.941 L 324.95 211.541 C 325.051 211.43 325.211 211.43 325.31 211.541 Z M 329.529 210.87 L 328.259 209.44 C 327.863 208.995 327.219 208.995 326.821 209.44 L 325.898 210.477 C 325.801 210.589 325.801 210.77 325.898 210.882 L 328.248 213.525 C 328.346 213.637 328.508 213.637 328.606 213.525 L 329.529 212.488 C 329.925 212.04 329.925 211.315 329.529 210.87 Z M 324.826 219.248 L 324.826 222.231 L 316.492 222.231 L 316.492 212.854 L 322.477 212.854 C 322.561 212.854 322.638 212.816 322.699 212.752 L 323.74 211.579 C 323.938 211.357 323.798 210.979 323.519 210.979 L 316.076 210.979 C 315.386 210.979 314.826 211.609 314.826 212.386 L 314.826 222.7 C 314.826 223.476 315.386 224.106 316.076 224.106 L 325.242 224.106 C 325.931 224.106 326.492 223.476 326.492 222.7 L 326.492 218.076 C 326.492 217.762 326.156 217.607 325.959 217.827 L 324.917 218.999 C 324.86 219.066 324.826 219.154 324.826 219.248 Z"
            ></path>
          </svg>
        </button>
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
      eventBus.$emit("updateForm", bookTitle);
    },
    toggleRead(bookTitle) {
      let index = this.library.findIndex(
        removedBook => removedBook.title === bookTitle
      );
      this.library[index].read = !this.library[index].read;
      eventBus.$emit("updateBook", bookTitle, this.library[index]);
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
#grid-container:nth-child(even) {
  background-color: #212e3c;
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
  margin: 0;
  text-decoration: none;
  font-family: sans-serif;
  cursor: pointer;
  background: transparent;
  transition: 150ms ease-in-out;
}
.btn-delete {
  border: 1px red solid;
  border-radius: 2px;
  color: red;
  &:hover {
    background-color: red;
    color: black;
    transform: scale(1.1);
  }
}
.btn-edit {
  border: none;
  fill: white;
  transition: 150ms ease-in-out;
  &:hover {
    transform: scale(1.2);
  }
}
input[type="checkbox"] {
  &:hover {
    cursor: pointer;
  }
}
</style>
