<template>
  <div class="home">
    <Page :books="books"></Page>
  </div>
</template>

<script>
// @ is an alias to /src
import Page from "@/components/Page.vue";

export default {
  name: "Home",
  components: {
    Page,
  },
  props: {
    books: Array,
  },
};
</script>
