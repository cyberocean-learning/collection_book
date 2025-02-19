<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="12" md="8">
        <h2>📚 Book Manager</h2>

        <!-- Book Form -->
        <v-card class="pa-4 mb-4">
          <v-text-field v-model="newBook.title" label="Title" required></v-text-field>
          <v-text-field v-model="newBook.author" label="Author" required></v-text-field>
          <v-text-field v-model="newBook.genre" label="Genre"></v-text-field>
          <v-text-field v-model="newBook.year" label="Publication Year" type="number"></v-text-field>
          <v-select v-model="newBook.rating" :items="[1, 2, 3, 4, 5]" label="Rating (1-5)"></v-select>
          <v-btn color="primary" @click="addBook">Add Book</v-btn>
        </v-card>

        <!-- Filters -->
        <v-card class="pa-4 mb-4">
          <h3>📌 Filter Books</h3>
          <v-select v-model="filterGenre" :items="uniqueGenres" label="Filter by Genre"></v-select>
          <v-select v-model="filterYear" :items="uniqueYears" label="Filter by Year"></v-select>
          <v-select v-model="filterReadStatus" :items="['All', 'Read', 'Unread']" label="Filter by Read Status"></v-select>
          <v-select v-model="filterRating" :items="[0, 1, 2, 3, 4, 5]" label="Filter by Rating (0-5)"></v-select>
        </v-card>

        <!-- Book List -->
        <v-list>
          <v-list-item v-for="(book, index) in filteredBooks" :key="index">
            <v-list-item-content>
              <v-list-item-title>
                <strong>{{ book.title }}</strong> by {{ book.author }} ({{ book.year }})
              </v-list-item-title>
              <v-list-item-subtitle>
                Genre: {{ book.genre }} - Rating: {{ book.rating }} ⭐
              </v-list-item-subtitle>
              <!-- Smaller & Narrower Read/Unread Button -->
              <v-btn x-small :color="book.read ? 'green' : 'red'" @click="toggleRead(index)" 
  style="width: auto; max-width: 40px; padding: 2px 8px;">
    {{ book.read ? "Read" : "Unread" }}
  </v-btn>

            </v-list-item-content>
            <v-list-item-action>
              <v-btn icon @click="editBook(index)"><v-icon>mdi-pencil</v-icon></v-btn>
              <v-btn icon @click="deleteBook(index)"><v-icon>mdi-delete</v-icon></v-btn>
            </v-list-item-action>
          </v-list-item>
        </v-list>

        <!-- Edit Dialog -->
        <v-dialog v-model="editDialog" max-width="400px">
          <v-card class="pa-4">
            <h3>Edit Book</h3>
            <v-text-field v-model="editedBook.title" label="Title"></v-text-field>
            <v-text-field v-model="editedBook.author" label="Author"></v-text-field>
            <v-text-field v-model="editedBook.genre" label="Genre"></v-text-field>
            <v-text-field v-model="editedBook.year" label="Publication Year" type="number"></v-text-field>
            <v-select v-model="editedBook.rating" :items="[1, 2, 3, 4, 5]" label="Rating (1-5)"></v-select>
            <v-btn color="primary" @click="saveEdit">Save</v-btn>
            <v-btn color="secondary" @click="editDialog = false">Cancel</v-btn>
          </v-card>
        </v-dialog>

      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      newBook: { title: "", author: "", genre: "", year: "", rating: 1, read: false },
      books: [],
      editDialog: false,
      editedBook: {},
      editedIndex: null,

      // Filters
      filterGenre: "All",
      filterYear: "All",
      filterReadStatus: "All",
      filterRating: 0
    };
  },
  computed: {
    uniqueGenres() {
      return ["All", ...new Set(this.books.map(book => book.genre))];
    },
    uniqueYears() {
      return ["All", ...new Set(this.books.map(book => book.year))].sort((a, b) => b - a);
    },
    filteredBooks() {
      return this.books.filter(book => {
        return (
          (this.filterGenre === "All" || book.genre === this.filterGenre) &&
          (this.filterYear === "All" || book.year == this.filterYear) &&
          (this.filterReadStatus === "All" ||
            (this.filterReadStatus === "Read" && book.read) ||
            (this.filterReadStatus === "Unread" && !book.read)) &&
          (this.filterRating === 0 || book.rating === this.filterRating)
        );
      });
    }
  },
  mounted() {
    this.books = JSON.parse(localStorage.getItem("books")) || [];
  },
  methods: {
    addBook() {
      if (this.newBook.title && this.newBook.author) {
        this.books.push({ ...this.newBook });
        this.saveBooks();
        this.newBook = { title: "", author: "", genre: "", year: "", rating: 1, read: false };
      }
    },
    deleteBook(index) {
      this.books.splice(index, 1);
      this.saveBooks();
    },
    editBook(index) {
      this.editedBook = { ...this.books[index] };
      this.editedIndex = index;
      this.editDialog = true;
    },
    saveEdit() {
      if (this.editedIndex !== null) {
        this.books[this.editedIndex] = { ...this.editedBook };
        this.saveBooks();
        this.editDialog = false;
      }
    },
    toggleRead(index) {
      this.books[index].read = !this.books[index].read;
      this.saveBooks();
    },
    saveBooks() {
      localStorage.setItem("books", JSON.stringify(this.books));
    }
  }
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
  text-align: center;
}
</style>
