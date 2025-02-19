<template>
    <v-row>
      <v-col cols="12" md="4">
        <v-select
          v-model="genre"
          :items="genres"
          label="Filter by Genre"
          clearable
          @change="$emit('filter-genre', genre)"
        ></v-select>
      </v-col>
      <v-col cols="12" md="4">
        <v-select
          v-model="rating"
          :items="[1, 2, 3, 4, 5]"
          label="Filter by Rating"
          clearable
          @change="$emit('filter-rating', rating)"
        ></v-select>
      </v-col>
      <v-col cols="12" md="4">
        <v-select
          v-model="readStatus"
          :items="readStatusOptions"
          label="Filter by Read Status"
          clearable
          @change="$emit('filter-read-status', readStatus)"
        ></v-select>
      </v-col>
    </v-row>
  </template>
  
  <script>
  export default {
    data() {
      return {
        genre: null,
        rating: null,
        readStatus: null,
        genres: ["Fiction", "Non-Fiction", "Technical", "Fantasy", "Mystery"],
        readStatusOptions: [
          { text: "Read", value: true },
          { text: "Unread", value: false },
        ],
      };
    },
  };
  </script>