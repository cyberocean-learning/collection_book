<template>
    <v-form @submit.prevent="submitForm">
      <v-text-field v-model="book.title" label="Title" required></v-text-field>
      <v-text-field v-model="book.author" label="Author" required></v-text-field>
      <v-select
        v-model="book.genre"
        :items="genres"
        label="Genre"  
        required
      ></v-select>
      <v-text-field
        v-model="book.year"
        label="Publication Year"
        type="number"  
        required
      ></v-text-field>  
      <v-rating v-model="book.rating" label="Rating"></v-rating>
      <v-checkbox v-model="book.isRead" label="Read"></v-checkbox>
      <v-btn type="submit" color="primary">{{ isEditMode ? "Update" : "Add" }} Book</v-btn>
    </v-form>
  </template>
  
  <script>
  export default {
    props: {
      bookToEdit: {
        type: Object,
        default: null,
      },
    },
    data() {
      return { 
        book: this.bookToEdit || {  
          title: "",
          author: "",
          genre: "",
          year: "",
          rating: 3,
          isRead: false,
        },
        genres: ["Fiction", "Non-Fiction", "Technical", "Fantasy", "Mystery"],
      };
    },
    computed: {
      isEditMode() {   
        return !!this.bookToEdit;
      },
    },
    methods:  {
      submitForm () {
        this.$emit(this.isEditMode ? "edit-book" : "add-book", this.book);
        if (!this.isEditMode) {
          this.resetForm();
        }
      },
      resetForm()  {
        this.book = {
          title: "",
          author: "",
          genre: "",
          year: "",
          rating: 3,
          isRead: false,
        };
      },
    },
  };
  </script>