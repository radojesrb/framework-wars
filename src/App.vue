<template>
  <div class="container" id="framework-wars">

    <!-- sort buttons -->
    <button type="button" class="btn btn-secondary"
    v-bind:class="{ active: activeSort == 'starCount' }"
    v-on:click="sort('starCount')">sort by stars</button>

    <button type="button" class="btn btn-secondary"
    v-bind:class="{ active: activeSort == 'forksCount' }"
    v-on:click="sort('forksCount')">sort by forks</button>

    <button type="button" class="btn btn-secondary"
    v-bind:class="{ active: activeSort == 'watchers' }"
    v-on:click="sort('watchers')">sort by watchers</button>

    <!-- framewors will be rendered with this code -->
    <div class="items">
      <framework-box v-for="(githubUser, index) in githubUsers"
      :apiurl="apiBaseUrl + githubUser" :index="index"></framework-box>
    </div>
  </div>
</template>

<script>
import frameworkBox from './components/framework-box/framework-box.vue';
import logo from './components/logo/logo.vue';

export default {
  name: 'framework-wars',
  components: {
    frameworkBox
  },
  data() {
    return {
      githubUsers: [
        'vuejs/vue',
        'angular/angular.js',
        'facebook/react',
        'emberjs/ember.js',
        'knockout/knockout',
        'jashkenas/backbone',
        'meteor/meteor'
      ],
      activeSort: '',
      apiBaseUrl: 'https://api.github.com/repos/'
    }
  },
  methods: {
    sort(dataSort) {
      this.activeSort = dataSort;
      this.githubUsers = [];
      const githubUsers = [];
      this.$children.sort(function(a, b) {
        return (a[dataSort] < b[dataSort]) ? 1 : ((b[dataSort] > a[dataSort]) ? -1 : 0);}
      );
      this.$children.forEach((child) => {
        githubUsers.push(child.fullName);
      });
      // reassign the list content after the next DOM update cycle
      this.$nextTick(() => {
        this.githubUsers = githubUsers;
      });
    }
  }
};
</script>

<style scoped lang="scss">
.container {
  margin-top: 8px;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;

  .items {
    margin-top: 8px;
  }
}
</style>
