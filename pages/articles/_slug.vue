<template>
  <div>
    <v-breadcrumbs :items="items" divider=">"></v-breadcrumbs>
    <v-row>
      <v-col
        cols="12"
        md="6"
        lg="4"
      >
        <v-card
          class="mx-auto"
          max-width="400"
          v-if="article.length > 0"
        >
          <v-img
            class="white--text align-end"
            height="200px"
            :src="api_url+article[0].thumbnail.url"
          >
            <v-card-title>{{ article[0].name }}</v-card-title>
          </v-img>

          <v-card-subtitle class="pb-0">{{ article[0].category.name }}</v-card-subtitle>

          <v-card-text
            v-html="$md.render(article[0].body)"
            class="text--primary tesaja"
          >
          </v-card-text>

          <v-card-actions>
            <v-btn
              color="orange"
              text
            >
              Share
            </v-btn>

            <v-btn
              color="orange"
              text
            >
              Explore
            </v-btn>
          </v-card-actions>
        </v-card>

        <v-card
          class="mx-auto"
          max-width="400"
          v-else
          loading
        >
          <v-card-title>Please Wait</v-card-title>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      article: [],
      dataParamId: '',
      api_url: process.env.baseUrl,
      items: [
        {
          text: 'Home',
          disabled: false,
          href: '/'
        },
        {
          text: 'Articles',
          disabled: false,
          href: '/articles'
        },
        {
          text: this.$route.params.slug,
          disabled: false,
          href: '/articles/' + this.$route.params.slug
        }
      ]
    }
  },
  mounted () {
    this.getSpecificData()
  },
  methods: {
    getID () {
      this.dataParam = this.$route.params.id
    },
    getSpecificData () {
      axios.get(this.api_url + '/articles', {
        params: {
          slug: this.$route.params.slug
        }
      })
        .then((response) => {
          this.article = response.data
        })
        .catch((error) => {
          console.log(error.response)
        })
    }
  }
}
</script>

<style scoped>
.tesaja /deep/ img {
  max-width: 100%;
}
</style>
