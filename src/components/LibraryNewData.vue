<template>
  <div class="library-input">
    <div class="error-msg" v-if="isError">
      <p>{{ error }}</p>
    </div>
    <form>
      <label for="author" class="form-author">Author's Name</label>
      <input
        type="text"
        name="author"
        id="author"
        class="author"
        placeholder="Author Name"
        v-model="author"
      />
      <label for="title" class="form-title">Title of Book</label>
      <input
        type="text"
        name="title"
        id="title"
        class="title"
        placeholder="Title of Book"
        v-model="title"
      />
      <label for="page-number" class="form-pages">Number of Pages</label>
      <input
        type="number"
        name="pages"
        id="page-number"
        class="pages"
        placeholder="0"
        v-model="pageNumber"
      />
      <label for="have-read" class="form-book-read">I have read this book</label>
      <input
        type="checkbox"
        name="read"
        id="have-read"
        class="book-read"
        v-model="haveRead"
      />
      <button @click.prevent="addToLibrary">Add Book</button>
      <button @click.prevent="cancelNewBook">Cancel</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "LibraryData",
  data() {
    return {
      author: "",
      title: "",
      pageNumber: 0,
      haveRead: false,
      isError: false,
      error: ""
    };
  },
  methods: {
    addToLibrary() {
      if (this.isAcceptedInput()) {
        let library = this.getLibrary();
        localStorage.setItem("library", JSON.stringify(library));
        this.$emit("update:visible", false);
      }
    },
    getLibrary() {
      let tempLibrary = JSON.parse(localStorage.getItem("library")) || [];
      tempLibrary.unshift({
        author: this.author,
        title: this.title,
        pages: this.pageNumber,
        read: this.haveRead
      });
      return tempLibrary;
    },
    isAcceptedInput() {
      if (this.author === "" || this.title === "") {
        this.setError("Author and Title cannot be blank!");
        return false;
      }
      if (isNaN(Number(this.pageNumber)) || Number(this.pageNumber) <= 0) {
        this.setError("The book cannot be 0 pages!");
        return false;
      }
      this.isError = false;
      return true;
    },
    setError(message) {
      this.isError = true;
      this.error = message;
    },
    cancelNewBook() {
      this.$emit("update:visible", false);
    }
  }
};
</script>

<style lang="scss" scoped>
form {
  display: inline-flex;
  flex-direction: column;
  input {
    margin-bottom: 10px;
  }
  #page-number {
    width: 50px;
    align-self: center;
  }
  input[type="number"] {
    -moz-appearance: textfield;
  }
  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
  button {
    width: 150px;
    align-self: center;
    margin-bottom: 15px;
  }
}
</style>
