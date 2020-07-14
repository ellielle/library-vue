<template>
  <div>
    <h1 id="library-title" v-if="!titleCanBeChanged" @click="changeTitle">
      {{ libraryTitle }}
    </h1>
    <input
      type="text"
      name="header"
      id="header-title"
      ref="title"
      placeholder="Enter a new title"
      v-if="titleCanBeChanged"
      @keypress.enter="setNewTitle"
      @keydown.delete.esc="cancelTitle"
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
  },
  destroyed() {
    eventBus.$off("formVisible");
    eventBus.$off("addToLibrary");
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
      this.titleCanBeChanged = !this.titleCanBeChanged;
      this.$nextTick(() => {
        this.$refs.title.focus();
      });
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
    }
  },
  components: {
    "library-new-data": LibraryNewData,
    "library-data": LibraryData
  }
};
</script>
<style lang="scss">
#library-title {
  margin: auto 300px;
  &:hover {
    cursor: pointer;
  }
}
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
