<template>
  <transition name="fade">
    <div class="wrapper" v-bind:class="{ active: isVue }">
      <div class="row no-gutters row-eq-height">
        <div class="number col-md-1">
          {{ parseInt(index) + 1 }}
        </div>
        <div class="logo col-md-2">
          <img :src="avatarSrc" class="avatar">
        </div>
        <div class="data col-md-9">
          <h1 class="title">{{ name }}</h1>
          <table class="table table-striped">
            <thead>
              <tr>
                <th><img src="./assets/star.svg" /> stars</th>
                <th><img src="./assets/fork.svg" /> forks</th>
                <th><img src="./assets/watcher.svg" /> watchers</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <th>{{ starCount }}</th>
                <td>{{ forksCount }}</td>
                <td>{{ watchers }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
module.exports = {
  data () {
    return {
      name: '',
      avatarSrc: '',
      starCount: '',
      forksCount: '',
      watchers: '',
      fullName: '',
      isVue: false
    }
  },
  props: [
    'apiurl',
    'index'
  ],
  mounted() {
    this.$http.get(this.apiurl).then((response) => {
      this.updateData(response.body);
    }, (response) => {
      // error callback
    });
  },
  methods: {
    updateData(data) {
      this.name = data.name;
      this.fullName = data.full_name;
      this.starCount = data.stargazers_count;
      this.forksCount = data.forks_count;
      this.watchers = data.watchers_count;
      this.avatarSrc = data.owner.avatar_url;

      if (data.name === 'vue') {
        this.isVue = true;
      }
    }
  }
};
</script>

<style scoped lang="scss">
// color pallete
$active-row-background-color:   #C8C8C8;
$active-row-text-color:   #FFFFFF;
$row-background-color: #EEEEEE;

.wrapper {
  margin-bottom: 8px;

  &.active {
    .row {
      background-color: $active-row-background-color;
    }
  }

  .data {
    font-size: 16px;
    font-weight: 400;
    @media screen and (max-width: 768px) {
      text-align: center;
    }
  }
  .number {
    font-size: 16px;
    font-weight: 600;
    text-align: center;
  }
  .avatar {
    display: block;
    margin: 0 auto;
    width: 50%;
  }
  .title {
    font-size: 22px;
    text-transform: capitalize;
    margin: 0 0 10px 0;
  }
  .table {
    td, th {
      border: 0;
    }
  }
}

.row {
  margin-bottom: 0;
  border-radius: .25rem;
  border: 1px solid $active-row-background-color;
  background-color: $row-background-color;

  & > [class^="col-"], & > [class*=" col-"] {
    background-color: transparent;
    border: 0;
  }
}
</style>
