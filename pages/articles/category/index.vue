<template>
  <div>
    <v-breadcrumbs :items="items" divider=">"></v-breadcrumbs>
    <v-row>
      <v-col>
        <h4>ini index category</h4>
      </v-col>
    </v-row>

    <v-row>
      <v-col>
        <ul
          v-for="item in allCategories"
          :key="item.id"
        >
          <v-btn
            :to="'/articles/category/' + item.name"
            class="mb-2"
          >
            {{ item.name }}
          </v-btn>
        </ul>
      </v-col>
    </v-row>

  </div>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      api_url: process.env.baseUrl,
      allCategories: [],
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
          text: 'Cateogry',
          disabled: false,
          href: '/articles/category'
        }
      ]
    }
  },
  created () {
    this.getCategoriesData()
  },
  methods: {
    getCategoriesData () {
      axios.get(this.api_url + '/categories')
        .then((response) => {
          this.allCategories = response.data
        })
        .catch((error) => {
          console.log(error.response)
        })
    }
  }
}
</script>

<style>

</style>
