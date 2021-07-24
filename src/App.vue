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
    <button class="btn" @click="prev">前へ</button>
    <button class="btn" @click="next">次へ</button>

    <section class="error" v-if="errored">
      <p>
        We're sorry, we're not able to retrieve this booksrmation at the moment,
        please try back later
      </p>
    </section>

    <router-view :books="books"></router-view>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      books: [],
      keyword: null,
      pageCount: 1,
      errored: false,
    };
  },
  watch: {
    pageCount() {
      this.search();
    },
  },
  methods: {
    async search() {
      const baseURL =
        "https://app.rakuten.co.jp/services/api/BooksTotal/Search/20170404";
      const bookParams = {
        applicationId: "1031111983337341334",
        booksGenreId: "001",
        hits: 20,
        page: this.pageCount,
        keyword: this.keyword,
      };
      await axios
        .get(baseURL, { params: bookParams })
        .then((res) => {
          this.books = [];
          console.log(this.books);
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
    prev() {
      this.pageCount <= 1 ? (this.pageCount = 20) : this.pageCount--;
    },
    next() {
      this.pageCount >= 20 ? (this.pageCount = 1) : this.pageCount++;
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
.btn {
  border: solid 1px black;
  background-color: aqua;
  color: #fff;
  font-weight: bold;
  margin: 2px;
}
</style>
