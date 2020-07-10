<template>
  <div>
    <h1 id="library-title" v-if="!titleCanBeChanged" @click="changeTitle">
      {{ libraryTitle }}
    </h1>
    <input
      type="text"
      name="header"
      id="header-title"
      v-if="titleCanBeChanged"
      @keypress.enter="setNewTitle"
    />
    <hr />
    <button @click="toggleNewForm" v-if="!visible">Add New Book</button>
    <div>
      <library-new-data
        v-if="visible"
        :visible.sync="visible"
      ></library-new-data>
    </div>
    <library-list v-if="!visible" :library="library"></library-list>
  </div>
</template>

<script>
import LibraryNewData from "./LibraryNewData.vue";
import LibraryList from "./LibraryList.vue";
export default {
  name: "LibraryMain",
  data() {
    return {
      visible: false,
      titleCanBeChanged: false,
      libraryTitle: "",
      library: []
    };
  },
  created() {
    if (localStorage.getItem("libraryTitle")) {
      this.libraryTitle = localStorage.getItem("libraryTitle");
    } else {
      this.libraryTitle = "My Library";
    }
    this.library = this.loadLibraryFromStorage();
  },
  methods: {
    toggleNewForm() {
      this.visible = !this.visible;
    },
    changeTitle() {
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
      return JSON.parse(localStorage.getItem("library"));
    }
  },
  components: {
    "library-new-data": LibraryNewData,
    "library-list": LibraryList
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
</style>
