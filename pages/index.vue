<template lang="html">
  <div>
    <v-text-field v-model="query" label="Search"></v-text-field>
    <v-btn
      color="primary"
      elevation="2"
      rounded
      outlined
      @click="handleSearchManga"
      >Search</v-btn
    >
    <v-divider class="mt-2 mb-2"></v-divider>

    <div class="cartoonRender d-flex flex-wrap">
      <v-card
        v-for="manga in results"
        :key="manga.id"
        class="my-2 mx-2"
        max-width="370"
        outlined
        @click="handleMangaClick(manga)"
      >
        <v-list-item three-line>
          <v-list-item-content>
            <div class="overline mb-4">{{ manga.type }}</div>
            <v-list-item-title class="headline mb-1">
              {{ manga.title }}
            </v-list-item-title>
            <v-list-item-subtitle>{{ manga.synopsis }}</v-list-item-subtitle>
          </v-list-item-content>

          <img
            :src="manga.image_url"
            :style="{
              width: '150px',
              height: '100%',
              marginTop: '10px',
              marginLeft: '15px',
              borderRadius: '10px',
            }"
            alt=""
          />
        </v-list-item>
        <v-card-actions>
          <v-btn block outlined text :href="manga.url" target="_blank">
            Button
          </v-btn>
        </v-card-actions>
      </v-card>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  // asyncData(ctx) {
  //   console.log(ctx.route.query.q)
  //   if (ctx.route.query.q === undefined) {
  //     ctx.route.query.q = 'Dragon Ball'
  //   }
  //   const url = `https://api.jikan.moe/v3/search/manga?q=${ctx.route.query.q}&page=1`
  //   return axios.get(url).then((res) => {
  //     return {
  //       query: ctx.route.query.q,
  //       results: res.data.results,
  //     }
  //   })
  // },
  data() {
    return {
      query: 'Dragon',
      results: [],
    }
  },
  async fetch() {
    const q = this.$route.query.q
    this.query = q
    console.log(q)
    if (q && q.length >= 3) {
      const url = `https://api.jikan.moe/v3/search/manga?q=${q}&page=1`
      const response = await axios.get(url)
      this.results = response.data.results
    }
  },
  methods: {
    async handleSearchManga() {
      this.$router.replace({ name: 'index', query: { q: this.query } })
      const url = `https://api.jikan.moe/v3/search/manga?q=${this.query}&page=1`
      const response = await axios.get(url)
      this.results = response.data.results
    },
    handleMangaClick(manga) {
      console.log(manga)
    },
  },
}
</script>

<style lang="css" scoped></style>
