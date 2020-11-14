<template>
  <div id="app" class="bg-lightWhite">
    <div class="container">
      <header class="flex justify-between py-6 lg:py-12 mb-6 lg:mb-12">
          <img src="@/assets/images/logo.svg" alt="Gemography Challenge"
            class="h-16 px-4"
          >
          <div class="filters flex items-center relative text-sm">
            <svg
              viewBox="0 0 24 24" width="16" height="16"
              stroke="currentColor" stroke-width="2" fill="none"
              stroke-linecap="round" stroke-linejoin="round"
            >
              <polygon points="22 3 2 3 10 12.46 10 19 14 21 14 12.46 22 3"></polygon>
            </svg>
            <span class="uppercase text-desc ml-2">order: </span>
            <span
              class="flex items-center uppercase font-bold ml-2 cursor-pointer"
              @click="changeSort"
            >
              <span class="mx-1">{{order}}</span>
              <svg v-if="order === 'asc'"
                viewBox="0 0 24 24" width="16" height="16"
                stroke="currentColor" stroke-width="2" fill="none"
                stroke-linecap="round" stroke-linejoin="round"
              >
                <polyline points="6 9 12 15 18 9"></polyline>
              </svg>
              <svg v-else
                viewBox="0 0 24 24" width="16" height="16"
                stroke="currentColor" stroke-width="2" fill="none"
                stroke-linecap="round" stroke-linejoin="round"
              >
                <polyline points="18 15 12 9 6 15"></polyline>
              </svg>
            </span>
          </div>
      </header>
      <section class="repos" v-if="repos.length > 0 && !loading">
        <div v-for="(repo, i) in repos" :key="'repo'+i+'id'+repo.id">
          <Repo :repo="repo" />
        </div>
        <infinite-loading @infinite="loadMore" :identifier="infiniteId">
          <div slot="spinner" class="flex items-center justify-center p-4">
            <div class="loading"></div>
          </div>
        </infinite-loading>
      </section>
      <div v-if="loading"
        class="fullScreen flex items-center justify-center"
      >
        <div class="loading" />
      </div>
      <BackToTop />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';
import InfiniteLoading from 'vue-infinite-loading';
import Repo from './components/Repo.vue';
import BackToTop from './components/BackToTop.vue';

export default {
  name: 'App',
  components: {
    Repo,
    BackToTop,
    InfiniteLoading,
  },
  data() {
    return {
      repos: [],
      page: 1,
      order: 'desc',
      loading: true,
      dateFilter: moment().subtract(30, 'days').format('YYYY-MM-DD'),
      infiniteId: +new Date(),
    };
  },
  methods: {
    getRepos(order) {
      // Get Repositories from Github
      axios.get(`https://api.github.com/search/repositories?q=created:>${this.dateFilter}&sort=stars&order=${order}&page=${this.page}&per_page=10`).then(({ data }) => {
        if (data && data.items.length > 0) {
          this.repos = data.items;
        }
        this.loading = false;
      }).catch((error) => {
        // eslint-disable-next-line
        console.log(error);
        this.loading = false;
      });
    },
    changeSort() {
      this.order = (this.order === 'asc') ? 'desc' : 'asc';
      this.infiniteId += 1;
      this.page = 1;
      this.getRepos(this.order);
    },
    loadMore($state) {
      this.page += 1;
      axios.get(`https://api.github.com/search/repositories?q=created:>${this.dateFilter}&sort=stars&order=${this.order}&page=${this.page}&per_page=10`).then(({ data }) => {
        if (data && data.items.length > 0) {
          this.repos.push(...data.items);
          $state.loaded();
        } else {
          $state.complete();
        }
      });
    },
  },
  mounted() {
    this.getRepos(this.order);
  },
};
</script>
