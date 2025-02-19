<template>
    <v-list-item>
      <v-list-item-content>
        <v-list-item-title>{{ book.title }}</v-list-item-title>
        <v-list-item-subtitle>{{ book.author }} ({{ book.year }})</v-list-item-subtitle>
        <v-list-item-subtitle>Genre: {{ book.genre }}</v-list-item-subtitle>
        <v-list-item-subtitle>Rating: {{ book.rating }}</v-list-item-subtitle>
        <v-list-item-subtitle>{{
          book.isRead ? "Read" : "Unread"
        }}</v-list-item-subtitle>
      </v-list-item-content>
      <v-list-item-action>
        <v-btn icon @click="$emit('edit-book', book)">
          <v-icon>mdi-pencil</v-icon>
        </v-btn>
        <v-btn icon @click="$emit('delete-book', book.id)">
          <v-icon>mdi-delete</v-icon>
        </v-btn>
      </v-list-item-action>
    </v-list-item>
  </template>
  
  <script>
  export default {
    props: {
      book: {
        type: Object,
        required: true,
      },
    },
  };
  </script>