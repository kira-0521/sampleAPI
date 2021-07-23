<template>
  <div>
    {{ info }}
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      info: null,
    };
  },
  mounted() {
    const baseURL =
      "https://app.rakuten.co.jp/services/api/BooksTotal/Search/20170404";
    const params = {
      applicationId: "1031111983337341334",
      booksGenreId: "002",
      keyword: "漫画",
    };
    axios
      .get(baseURL, { params: params })
      .then((res) => {
        this.info = res;
      })
      .catch((err) => console.log(err));
  },
};
</script>

<style>
h1 {
  font-size: 32px;
}
</style>
