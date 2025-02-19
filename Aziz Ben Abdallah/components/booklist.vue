<template>
    <div>
      <v-list>
        <BookItem
          v-for="book in books"
          :key="book.id"
          :book="book"
          @delete-book="$emit('delete-book', book.id)"
          @edit-book="$emit('edit-book', book)"
        />
      </v-list>
    </div>
</template>
  
<script>

  import BookItem from "./bookitem.vue" ;
  
  export default {
    components: { BookItem },
    props: {
      books: {
        type: Array,
        required: true,
      },
    },
  };

</script>