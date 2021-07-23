<template>
  <div>
    <h1>楽天から検索</h1>
    <input
      type="text"
      v-model="keyword"
      placeholder="検索キーワード"
      @keydown.enter="search"
    />
    <p>{{ pageCount }}ページ目</p>
    <router-link to="/page/:id" @click="prevPage">前へ</router-link>
    <router-link to="/page/:id" @click="nextPage">次へ</router-link>

    <section class="error" v-if="errored">
      <p>
        We're sorry, we're not able to retrieve this booksrmation at the moment,
        please try back later
      </p>
    </section>

    <Page v-else :books="books"></Page>
  </div>
</template>

<script>
import axios from "axios";
import Page from "./components/Page";

export default {
  components: {
    Page,
  },
  data() {
    return {
      books: [],
      keyword: null,
      pageCount: 1,
      errored: false,
    };
  },
  methods: {
    async search(event) {
      if (event.keyCode !== 13) return;
      const baseURL =
        "https://app.rakuten.co.jp/services/api/BooksTotal/Search/20170404";
      const params = {
        applicationId: "1031111983337341334",
        booksGenreId: "001",
        hits: 20,
        page: this.pageCount,
        keyword: this.keyword,
      };
      await axios
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
        });
    },
    prevPage() {
      if (this.pageCount <= 1) {
        this.pageCount = 20;
      } else {
        this.pageCount -= 1;
      }
    },
    nextPage() {
      if (this.pageCount >= 20) {
        this.pageCount = 1;
      } else {
        this.pageCount += 1;
      }
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
