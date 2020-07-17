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
      <label for="have-read" class="form-book-read">
        I have read this book
      </label>
      <input
        type="checkbox"
        name="read"
        id="have-read"
        class="book-read"
        v-model="haveRead"
      />
      <!-- Index is -1 when Add New Book is clicked, 0 or greater when editing a book -->
      <button @click.prevent="addToLibrary" v-if="this.index < 0">
        Add Book
      </button>
      <button @click.prevent="editBook" v-else>Edit Book</button>
      <button @click.prevent="closeForm">Cancel</button>
    </form>
  </div>
</template>

<script>
import { eventBus } from "../main";

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
  props: {
    index: {
      type: Number
    },
    library: {
      type: Array
    }
  },
  created() {
    if (this.index >= 0) {
      this.author = this.library[this.index].author;
      this.title = this.library[this.index].title;
      this.pageNumber = this.library[this.index].pages;
      this.haveRead = this.library[this.index].read;
    }
  },
  methods: {
    addToLibrary() {
      if (this.isAcceptedInput()) {
        let newBookEntry = this.getLibrary();
        eventBus.$emit("addToLibrary", newBookEntry);
        this.closeForm();
      }
    },
    closeForm() {
      eventBus.$emit("formVisible", "library-data");
    },
    editBook() {
      // Replace the current index with new information and
      if (this.isAcceptedInput()) {
        this.library[this.index] = this.getLibrary();
        eventBus.$emit("updateBook");
        this.closeForm();
      }
    },
    getLibrary() {
      return {
        author: this.author,
        title: this.title,
        pages: this.pageNumber,
        read: this.haveRead
      };
    },
    isAcceptedInput() {
      // Return true if fields are valid, otherwise false and show an error / prevent data from being sent
      if (this.author === "" || this.title === "") {
        this.setError("Author and Title cannot be blank!");
        return false;
      }
      if (isNaN(Number(this.pageNumber)) || Number(this.pageNumber) <= 0) {
        this.setError("The book must be a valid number of pages!");
        return false;
      }
      this.isError = false;
      return true;
    },
    setError(message) {
      this.isError = true;
      this.error = message;
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
    cursor: pointer;
  }
}
.error-msg {
  color: red;
}
input[type="checkbox"] {
  &:hover {
    cursor: pointer;
  }
}
</style>
