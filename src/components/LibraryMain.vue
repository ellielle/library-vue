<template>
  <div>
    <h1 id="library-title" v-if="!titleCanBeChanged" @click="changeTitle">
      {{ libraryTitle }}
      <svg viewBox="314.826 209.106 15 15" width="15" height="15">
        <path
          d="M 325.31 211.541 L 327.659 214.184 C 327.758 214.296 327.758 214.477 327.659 214.589 L 321.971 220.989 L 319.555 221.291 C 319.232 221.331 318.959 221.024 318.994 220.661 L 319.264 217.941 L 324.95 211.541 C 325.051 211.43 325.211 211.43 325.31 211.541 Z M 329.529 210.87 L 328.259 209.44 C 327.863 208.995 327.219 208.995 326.821 209.44 L 325.898 210.477 C 325.801 210.589 325.801 210.77 325.898 210.882 L 328.248 213.525 C 328.346 213.637 328.508 213.637 328.606 213.525 L 329.529 212.488 C 329.925 212.04 329.925 211.315 329.529 210.87 Z M 324.826 219.248 L 324.826 222.231 L 316.492 222.231 L 316.492 212.854 L 322.477 212.854 C 322.561 212.854 322.638 212.816 322.699 212.752 L 323.74 211.579 C 323.938 211.357 323.798 210.979 323.519 210.979 L 316.076 210.979 C 315.386 210.979 314.826 211.609 314.826 212.386 L 314.826 222.7 C 314.826 223.476 315.386 224.106 316.076 224.106 L 325.242 224.106 C 325.931 224.106 326.492 223.476 326.492 222.7 L 326.492 218.076 C 326.492 217.762 326.156 217.607 325.959 217.827 L 324.917 218.999 C 324.86 219.066 324.826 219.154 324.826 219.248 Z"
        ></path>
      </svg>
    </h1>
    <input
      type="text"
      name="header"
      id="header-title"
      ref="title"
      placeholder="Enter a new title"
      v-if="titleCanBeChanged"
      @keypress.enter="setNewTitle"
      @keydown.esc="cancelTitle"
    />
    <hr />
    <button @click="toggleNewForm">Add New Book</button>
    <component :is="visibleComponent" :library="boundData"></component>
  </div>
</template>

<script>
import LibraryNewData from "./LibraryNewData.vue";
import LibraryData from "./LibraryData";
import { eventBus } from "../main";

export default {
  name: "LibraryMain",
  data() {
    return {
      libraryUpdated: false,
      titleCanBeChanged: false,
      libraryTitle: "",
      library: [],
      visibleComponent: "library-data"
    };
  },
  created() {
    this.libraryTitle = localStorage.getItem("libraryTitle")
      ? localStorage.getItem("libraryTitle")
      : "My Library";
    this.library = this.loadLibraryFromStorage();
    eventBus.$on("formVisible", newVisibleComponent => {
      this.visibleComponent = newVisibleComponent;
    });
    eventBus.$on("addToLibrary", newEntry => {
      this.library.unshift(newEntry);
      this.updateSavedLibrary();
    });
    eventBus.$on("removeBookFromLibrary", bookTitle => {
      let bookIndex = this.findBookIndex(bookTitle);
      this.library.splice(bookIndex, 1);
      console.log(bookTitle, bookIndex);
      this.updateSavedLibrary();
    });
    eventBus.$on("updateBook", (bookTitle, updateData) => {
      let bookIndex = this.findBookIndex(bookTitle);
      this.library[bookIndex] = updateData;
      this.updateSavedLibrary();
    });
  },
  destroyed() {
    eventBus.$off("formVisible");
    eventBus.$off("addToLibrary");
    eventBus.$off("removeBookFromLibrary");
  },
  computed: {
    boundData() {
      if (this.visibleComponent === "library-data") {
        return this.library;
      } else {
        return {};
      }
    }
  },
  methods: {
    toggleNewForm() {
      this.visibleComponent = "library-new-data";
    },
    changeTitle() {
      if (this.visibleComponent === "library-data") {
        this.titleCanBeChanged = !this.titleCanBeChanged;
        this.$nextTick(() => {
          this.$refs.title.focus();
        });
      }
    },
    cancelTitle() {
      this.titleCanBeChanged = !this.titleCanBeChanged;
    },
    setNewTitle(event) {
      if (event.target.value !== "") {
        this.titleCanBeChanged = !this.titleCanBeChanged;
        this.libraryTitle = event.target.value;
        localStorage.setItem("libraryTitle", this.libraryTitle);
      }
    },
    loadLibraryFromStorage() {
      if (!JSON.parse(localStorage.getItem("library"))) {
        return this.createDefaultLibrary();
      } else {
        return JSON.parse(localStorage.getItem("library"));
      }
    },
    updateSavedLibrary() {
      localStorage.setItem("library", JSON.stringify(this.library));
    },
    createDefaultLibrary() {
      let newLibrary = [
        {
          author: "Sun Tzu",
          title: "The Art of War",
          pages: 260,
          read: false
        }
      ];
      localStorage.setItem("library", JSON.stringify(newLibrary));
      return newLibrary;
    },
    findBookIndex(bookTitle) {
      return this.library.findIndex(
        removedBook => removedBook.title === bookTitle
      );
    }
  },
  components: {
    "library-new-data": LibraryNewData,
    "library-data": LibraryData
  }
};
</script>
<style lang="scss" scoped>
#library-title {
  margin: auto 300px;
  &:hover {
    cursor: pointer;
    background: linear-gradient(90deg, #ff8a00, #e52e71);
    background-clip: text;
    -webkit-text-fill-color: transparent;
  }
}
svg {
  fill: #808080;
  margin-right: 0;
}
</style>
