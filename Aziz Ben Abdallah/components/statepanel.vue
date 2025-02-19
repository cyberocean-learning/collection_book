<template>
    <v-card>
      <v-card-title>Statistics</v-card-title>
      <v-card-text>
        <p>Total Books: {{ totalBooks }}</p>
        <p>Read Books: {{ readBooks }}</p>
        <p>Unread Books: {{ unreadBooks }}</p>
        <p>Average Rating: {{ averageRating.toFixed(2) }}</p>
      </v-card-text>
    </v-card>
  </template>
  
  <script>
  export default {
    props: {
      books: {
        type: Array,
        required: true,
      },
    },
    computed: {
      totalBooks() {
        return this.books.length;
      },
      readBooks() {
        return this.books.filter((book) => book.isRead).length;
      },
      unreadBooks() {
        return this.books.filter((book) => !book.isRead).length;
      },
      averageRating() {
        if (this.books.length === 0) return 0;
        const totalRating = this.books.reduce((sum, book) => sum + book.rating, 0);
        return totalRating / this.books.length;
      },
    },
  };
  </script>