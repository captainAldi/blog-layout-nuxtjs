<template>
  <div>
    <v-breadcrumbs :items="items" divider=">"></v-breadcrumbs>
    <v-row>
      <v-col
        cols="12"
      >
        <v-carousel
          cycle
          height="400"
          hide-delimiter-background
          show-arrows-on-hover
        >
          <v-carousel-item
            v-for="(slide, i) in slides"
            :key="i"
          >
            <v-sheet
              :color="colors[i]"
              height="100%"
            >
              <v-row
                class="fill-height"
                align="center"
                justify="center"
              >
                <div class="display-3">{{ slide }} Slide</div>
              </v-row>
            </v-sheet>
          </v-carousel-item>
        </v-carousel>
      </v-col>
    </v-row>

    <v-row>
      <v-col
        cols="12"
        lg="10"
        md="6"
      >
        <v-card
          tile
        >
          <v-card-title>Daftar Artikel</v-card-title>
          <v-list-item
            v-for="item in articles"
            :key="item.id"
            :to="'/articles/'+item.slug"
            link
          >
            <v-list-item-content>
              <v-list-item-title>{{ item.name }}</v-list-item-title>
              <!-- <v-lits-item-subtitle v-if="item.body.length < 20">
                {{ item.body }}
              </v-lits-item-subtitle>
              <v-lits-item-subtitle v-if="item.body.length >= 20" v-html="$md.render(item.body).substring(0,20)+'...'">
              </v-lits-item-subtitle> -->
              <v-list-item-subtitle>Category: {{item.category.name}}</v-list-item-subtitle>
            </v-list-item-content>
          <v-avatar
            class="ma-3"
            size="150px"
            tile
          >
            <v-img :src="api_url+item.thumbnail.url"></v-img>
          </v-avatar>
          </v-list-item>
        </v-card>

        <v-pagination
          v-model="page"
          :length="pageArticle"
          total-visible="6"
          @input="getData()"
        ></v-pagination>
      </v-col>

      <v-col
        cols="12"
        lg="2"
        md="6"
      >
        <v-card>
          <v-card-title>Kategori</v-card-title>
          <v-list-item
            v-for="item in allCategories"
            :key="item.id"
            link
            :to="'/articles/category/' + item.name"
          >
            <v-list-item-content>
              <v-list-item-title>{{ item.name }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-card-actions
          >
            <v-btn
              color="primay"
              v-if="categoriesCount > 2"
              to="/articles/category"
            >
              Lainnya
            </v-btn>
          </v-card-actions>
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
      articles: [],
      articlesCount: 0,
      allCategories: 0,
      categoriesCount: 0,
      colors: [
        'indigo',
        'warning',
        'pink darken-2',
        'red lighten-1',
        'deep-purple accent-4'
      ],
      slides: [
        'First',
        'Second',
        'Third',
        'Fourth',
        'Fifth'
      ],
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
        }
      ],
      page: 1
    }
  },
  created () {
    this.getArticlesCount()
    this.getCategoriesCount()
    this.getCategoriesData()
    this.getData()
  },
  computed: {
    pageArticle () {
      const datapage = this.articlesCount > 0 ? Math.ceil(this.articlesCount / 10) : this.articlesCount
      return datapage
    }
  },
  methods: {
    getArticlesCount () {
      axios.get(this.api_url + '/articles/count')
        .then((response) => {
          this.articlesCount = response.data
        })
    },
    getCategoriesCount () {
      axios.get(this.api_url + '/categories/count')
        .then((response) => {
          this.categoriesCount = response.data
        })
    },
    getCategoriesData () {
      axios.get(this.api_url + '/categories', {
        params: {
          _limit: '2'
        }
      })
        .then((response) => {
          this.allCategories = response.data
        })
        .catch((error) => {
          console.log(error.response)
        })
    },
    getData () {
      axios.get(this.api_url + '/articles', {
        params: {
          _limit: '10',
          _start: (this.page * 10) - 10
        }
      })
        .then((response) => {
          this.articles = response.data
        })
        .catch((error) => {
          console.log(error.response)
        })
    }
  }
}
</script>

<style scoped>
img {
  max-width: 20%;
  height: auto;
}
</style>
