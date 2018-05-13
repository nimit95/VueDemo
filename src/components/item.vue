<template>
  <main>
    <div class="top-card">
      <div class="section item">
        <a :href="item.url" target="_blank"><h2> {{ item.title || 'Loading item...' }}</h2></a>

      </div>
    </div>
  </main>
</template>

<script>
  export default {
    name: "item",
    data() {
      return {
        defaultText : "random article",
        topStoriesLink: "https://hacker-news.firebaseio.com/v0/topstories.json?print=pretty",
        topStories : [],
        itemId: 0,
        item: {}
      }
    },
    mounted() {
      this.getItemList();

      this.itemId = new Random
    },
    methods: {
      getItemList() {
        this.$http.get(this.topStoriesLink).then(function (response) {
          console.log(response);
          this.topStories = response.data;
        }, function (error) {
          console.log(error.statusText);
        })
      },
      getItem(itemId) {
        let itemLink = `https://hacker-news.firebaseio.com/v0/item/${itemId}.json?print=pretty`;

        this.$http.get(itemLink).then(function (response) {
          console.log("response", response);
          this.item = response.data;
        }, function (error) {
          console.log(error.statusText);
        })

      },
      getRandomNumber(max) {
        let min = 0;
        return Math.floor(Math.random() * max) + min
      }
    },
    watch: {
      topStories: function (stories) {
        console.log("Stories", stories);
        let randomNumber = this.getRandomNumber(stories.length - 1);
        console.log("randomNumber", randomNumber);
        this.getItem(randomNumber);

      }
    }
  }
</script>

<style scoped>

</style>
