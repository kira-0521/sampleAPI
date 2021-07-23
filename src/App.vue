<template>
  <div>
    <h1>楽天から検索</h1>
    <input
      type="text"
      v-model="keyword"
      placeholder="検索キーワード"
      @keydown.enter="search"
    />

    <section class="error" v-if="errored">
      <p>
        We're sorry, we're not able to retrieve this booksrmation at the moment,
        please try back later
      </p>
    </section>

    <section v-else>
      <div class="loading" v-if="loading">Loading...</div>

      <div class="books">
        <ul v-for="book in books" :key="book.index">
          <li>
            <h3>タイトル: {{ book.title }}</h3>
            <img :src="book.img" alt="" />
          </li>
        </ul>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      books: [],
      keyword: null,
      errored: false,
      loading: true,
    };
  },
  methods: {
    search(event) {
      if (event.keyCode !== 13) return;
      const baseURL =
        "https://app.rakuten.co.jp/services/api/BooksTotal/Search/20170404";
      const params = {
        applicationId: "1031111983337341334",
        booksGenreId: "001",
        hits: 20,
        keyword: this.keyword,
      };
      axios
        .get(baseURL, { params: params })
        .then((res) => {
          res.data.Items.reduce((acc, cur) => {
            acc.push({
              title: cur.Item.title,
              img: cur.Item.mediumImageUrl,
            });
            return (this.books = acc);
          }, []);
        })
        .catch((err) => {
          console.log(err);
          this.errored = true;
        })
        .finally(() => (this.loading = false));
    },
  },
};
</script>

<style>
h1 {
  font-size: 32px;
}
ul {
  list-style: none;
}
</style>
