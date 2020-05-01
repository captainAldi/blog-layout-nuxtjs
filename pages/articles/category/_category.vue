<template>
  <div>
    <v-breadcrumbs :items="items" divider=">"></v-breadcrumbs>

    <v-row>
      <v-col>
        <h2>Daftar Artikel {{ this.$route.params.category }}</h2>
      </v-col>
    </v-row>

    <v-row
      v-if="category.length > 0"
    >
      <v-col
        cols="12"
        lg="4"
        md="6"
        v-for="item in category"
        :key="item.id"
      >
        <v-card
          class="pa-2"
          hover
        >
          <v-img
            class="white--text align-end"
            height="200px"
            :src="api_url+item.thumbnail.url"
          >
            <v-card-title>{{ item.name }}</v-card-title>
          </v-img>

          <v-card-subtitle class="pb-0">{{ item.category.name }}</v-card-subtitle>

          <v-card-actions>
            <v-btn
              color="orange"
            >
              Share
            </v-btn>

            <v-btn
              color="orange"
              :to="'/articles/' + item.slug"
            >
              Explore
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>

    <v-row
      v-else
    >
      <v-col>
        <v-card
          loading=""
        >
          <v-card-title>
            Please Wait
          </v-card-title>
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
      category: [],
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
          text: 'Category',
          disabled: false,
          href: '/articles/category'
        },
        {
          text: this.$route.params.category,
          disabled: false,
          href: '/articles/category/' + this.$route.params.category
        }
      ]
    }
  },
  mounted () {
    this.getSpecificData()
  },
  methods: {
    getSpecificData () {
      axios.get(this.api_url + '/articles', {
        params: {
          'category.name': this.$route.params.category
        }
      })
        .then((response) => {
          this.category = response.data
        })
        .catch((error) => {
          console.log(error.response)
        })
    }
  }
}
</script>

<style scoped>
.tes {
  background-color: green;
}
</style>
