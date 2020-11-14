<template>
  <div class="flex flex-col lg:flex-row flex-wrap items-center px-8 shadow hover:shadow-lg
  transition py-6 mt-4 mb-8 rounded-lg border border-CleanGray bg-white">
    <div class="w-full lg:w-1/5">
      <div class="p-3">
        <a :href='repo.owner.html_url' target='_blank' rel='noopener noreferrer'>
          <img
            :src="repo.owner.avatar_url"
            :alt="repo.owner.login"
            class="w-full h-full rounded-lg"
          >
        </a>
      </div>
    </div>
    <div class="w-full lg:w-4/5">
      <div class="p-3">
        <h2 class="font-semibold text-3xl capitalize text-mainColor">
          <a :href='repo.html_url' target='_blank' rel='noopener noreferrer'>
            {{repo.name}}
          </a>
        </h2>
        <p class="text-lg my-2">{{repo.description}}</p>
        <div class="flex flex-wrap">
          <span class="flex items-center">
            <svg viewBox="0 0 16 16" version="1.1" width="16" height="16">
              <path
                fill-rule="evenodd"
                d="M8 .25a.75.75 0 01.673.418l1.882 3.815 4.21.612a.75.75 0 01.416
                1.279l-3.046 2.97.719 4.192a.75.75 0 01-1.088.791L8 12.347l-3.766
                1.98a.75.75 0 01-1.088-.79l.72-4.194L.818 6.374a.75.75 0
                01.416-1.28l4.21-.611L7.327.668A.75.75 0 018 .25zm0 2.445L6.615 5.5a.75.75
                0 01-.564.41l-3.097.45 2.24 2.184a.75.75 0 01.216.664l-.528 3.084
                2.769-1.456a.75.75 0 01.698 0l2.77 1.456-.53-3.084a.75.75 0
                01.216-.664l2.24-2.183-3.096-.45a.75.75 0 01-.564-.41L8 2.694v.001z"
              />
            </svg>
            <span class="ml-2">{{calc(repo.stargazers_count)}}</span>
          </span>
          <span class="flex items-center mx-5">
            <svg height="16" viewBox="0 0 16 16" version="1.1" width="16" >
              <path
                fill-rule="evenodd"
                d="M8 1.5a6.5 6.5 0 100 13 6.5 6.5 0 000-13zM0 8a8 8 0 1116 0A8 8 0 010 8zm9 3a1
                1 0 11-2 0 1 1 0 012 0zm-.25-6.25a.75.75 0 00-1.5 0v3.5a.75.75 0 001.5 0v-3.5z"
              />
            </svg>
            <span class="ml-2">{{calc(repo.open_issues_count)}}</span>
          </span>
          <span class="text-mainColor">
            Submitted {{getDate(repo.created_at)}} By {{repo.owner.login}}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment';

export default {
  name: 'Repo',
  props: {
    repo: {
      type: Object,
      default: () => [],
    },
  },
  methods: {
    calc(n) {
      let x = n.toString().length;
      const d = 10 ** 1;
      x -= x % 3;
      return Math.round((n * d) / 10 ** x) / d + ' kMGTPE'[x / 3];
    },
    getDate(date) {
      return moment(date).fromNow();
    },
  },
};
</script>
