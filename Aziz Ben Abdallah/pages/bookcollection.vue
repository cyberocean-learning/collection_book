<template>
    <v-app>
      <v-container>
        <h1 class="text-h3 mb-6">My Book Collection</h1>
        <bookform @add-book="addBook" />
        <filtercontrols
          @filter-genre="setGenreFilter"
          @filter-rating="setRatingFilter"
          @filter-read-status="setReadStatusFilter"
        />       
        <booklist
          :books="filteredBooks" 
          @delete-book="deleteBook" 
          @edit-book="editBook" 
        />
        <statspanel :books="books" />
      </v-container>
    </v-app>
  </template>
  
  <script>
  import BookForm from "@/components/bookform.vue";
  import BookItem from "@/components/bookitem.vue";
  import BookList from "@/components/booklist.vue";
  import StatsPanel from "@/components/statepanel.vue";
  import FilterControls from "@/components/filtercontrols.vue";
  
  export default {
    components: { 
        BookForm,
        BookItem,
        BookList,
        StatsPanel,
        FilterControls
    },
    data() { 
      return {
        books: [   
          {    
            id: 1,      
            title: "The Vue Guide", 
            author: "Sarah Frontend",
            genre: "Technical", 
            year: 2023,
            rating: 4, 
            isRead: true,  
          },
        ],
        genreFilter: null,
        ratingFilter: null,
        readStatusFilter: null, 
      };
    },
    computed: { 
      filteredBooks() {  
        return this.books.filter((book) => {
          const matchesGenre = 
            !this.genreFilter || book.genre === this.genreFilter;   
          const matchesRating =
            !this.ratingFilter || book.rating >= this.ratingFilter;   
          const matchesReadStatus = 
            this.readStatusFilter === null ||  
            book.isRead === this.readStatusFilter;
          return matchesGenre && matchesRating && matchesReadStatus;
        });
      },
    },
    methods: { 
      addBook(newBook) {
        this.books.push({ ...newBook, id: this.books.length + 1 });
      },
      deleteBook(bookId) {
        this.books = this.books.filter((book) => book.id !== bookId);
      },
      editBook(updatedBook) {
        const index = this.books.findIndex((book) => book.id === updatedBook.id);
        if (index !== -1) {
          this.books.splice(index, 1, updatedBook);
        }
      },
      setGenreFilter(genre) {
        this.genreFilter = genre;
      },
      setRatingFilter(rating) {
        this.ratingFilter = rating;
      },
      setReadStatusFilter(status) {
        this.readStatusFilter = status;
      },
    },
  };
  </script>