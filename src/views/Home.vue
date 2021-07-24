<template>
  <div class="home">
    <Page :books="books"></Page>
    <!-- search発動と同時にネストされたコンポーネントへのパスを通す必要がある？ -->
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
