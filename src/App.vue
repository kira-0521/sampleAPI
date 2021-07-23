<template>
  <div>
    <h1>楽天から検索</h1>

    <section class="error" v-if="errored">
      <p>
        We're sorry, we're not able to retrieve this information at the moment,
        please try back later
      </p>
    </section>

    <section v-else>
      <div class="loading" v-if="loading">Loading...</div>

      <div class="books">
        {{ info }}
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      info: [],
      errored: false,
      loading: true,
    };
  },
  mounted() {
    const baseURL =
      "https://app.rakuten.co.jp/services/api/BooksTotal/Search/20170404";
    const params = {
      applicationId: "1031111983337341334",
      booksGenreId: "001",
      hits: 20,
    };
    axios
      .get(baseURL, { params: params })
      .then((res) => {
        res.data.Items.reduce((acc, cur) => {
          acc.push(cur.Item.title);
          return (this.info = acc);
        }, []);
        console.log(this.info);
      })
      .catch((err) => {
        console.log(err);
        this.errored = true;
      })
      .finally(() => (this.loading = false));
  },
};
</script>

<style>
h1 {
  font-size: 32px;
}
</style>
