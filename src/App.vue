<template>
  <div id="app">
    <h1>GitHub Repo Lookup</h1>
    <search-box @keywordChanged="onKeywordChange"/>
    <hr>
    <div class="d-flex flex-wrap align-items-stretch" ref="repoDoms">
      <template v-for="(repo, index) in searchResult">
        <repo-item :repo="repo" :key="index"/>
      </template>
    </div>
    <div v-show="isLoading" class="text-center">
      <div class="spinner-border">
        <span class="sr-only">Loading...</span>
      </div>
      <p>Loading...</p>
    </div>
    <Observer @intersect="getNextPage" />
  </div>
</template>

<script>
import axios from 'axios';
import SearchBox from './components/SearchBox.vue';
import RepoItem from './components/RepoItem.vue';
import Observer from './components/Observer.vue';

export default {
  name: 'app',
  components: {
    SearchBox,
    RepoItem,
    Observer,
  },
  data() {
    return {
      isLoading: false,
      page: 1,
      totalPage: 1,
      keyword: '',
      searchResult: [],
    };
  },
  methods: {
    onKeywordChange(newKeyword) {
      this.searchResult = [];
      this.page = 1;
      this.doSearch(newKeyword);
      this.keyword = newKeyword;
    },
    async doSearch(keyword, page) {
      this.isLoading = true;
      const queryPage = page || 1;
      const result = await axios.get('https://api.github.com/search/repositories', {
        params: {
          q: keyword,
          page: queryPage,
        },
      });
      if (queryPage === 1) {
        this.totalPage = this.getLastPageNumber(result.headers.link);
      }
      this.searchResult = this.searchResult.concat(result.data.items);

      this.isLoading = false;
    },
    getNextPage() {
      if (this.keyword !== '' && this.page < this.totalPage) {
        this.page = this.page + 1;
        this.doSearch(this.keyword, this.page);
      }
    },
    getLastPageNumber(headersLink) {
      return headersLink.split(',')
        .map(link => link.split(';')[0].match(/page=([0-9]+)>$/)[1])
        .map(pageStr => Number.parseInt(pageStr, 10))
        .sort((a, b) => b - a)[0];
    },
  },
};
</script>
<style lang="stylus">
#app
  margin: 2rem 4rem;
</style>
