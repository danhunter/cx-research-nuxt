<template>
   <v-content>
    <v-container fluid grid-list-md>
      <v-layout row wrap>
        <v-flex
          xs12
          md6
          lg3
          v-for="(article, index) in articles"
          :key="index"
        >
          <v-card
            class="mx-auto"
            max-width="400">

            <v-img
              class="white--text align-end"
              height="200px"
              :src="article.urlToImage">
              <v-card-title>{{ article.title }}</v-card-title>
            </v-img>

            <v-card-subtitle class="pb-0">{{ article.source.name }}</v-card-subtitle>

            <v-card-text class="text--primary">
              <div>{{ article.description }}</div>
            </v-card-text>

            <v-card-actions>
              <v-btn
                color="orange"
                text
                target="_blank"
                :href="article.url">
                Explore
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </v-content>
</template>


<script>
  export default {
    mounted() {
      this.$OneSignal.push(() => {
        this.$OneSignal.isPushNotificationsEnabled((isEnabled) => {
          if (isEnabled) {
            console.log('Push notifications are enabled!')
          } else {
            console.log('Push notifications are not enabled yet.')
          }
        })
      })
    },
    async asyncData({ app }) {
      const { articles } = await app.$axios.$get(
        `https://newsapi.org/v2/top-headlines?sources=cnn&apiKey=${
            process.env.NEWS_API_KEY
        }`
      );

      return { articles };
    },
  };
</script>
