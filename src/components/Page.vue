<template>
  <section class="book">
    <div class="books">
      <ul v-for="book in books" :key="book.index">
        <li>
          <h3>{{ book.title }}</h3>
          <img :src="book.img" alt="" />
        </li>
      </ul>
    </div>
  </section>
</template>

<script>
export default {
  props: {
    books: Array,
  },
};
</script>

<style></style>
