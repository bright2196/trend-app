<template>
  <div>
    <div class="px-4 py-4 my-4 text-center">
      <svg
        width="269"
        height="82"
        viewBox="0 0 269 82"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          d="M1.6 16.24H15.92V40H22.88V16.24H37.2V10.64H1.6V16.24ZM89.2878 37L82.8478 28.84C89.0878 27.72 91.7278 24.96 91.7278 20C91.7278 13.32 86.8878 10.64 74.8478 10.64H56.8078V40H63.6878V29.4H74.8478H75.2878L83.6878 40H91.6878L91.4478 39.68L89.2878 37ZM63.8078 15.6H76.0478C81.8078 15.6 84.1278 16.84 84.1278 20C84.1278 23.16 81.8078 24.4 76.0478 24.4H63.8078V15.6ZM112.212 10.64V40H148.172V34.4H119.172V27.8H141.092V22.2H119.172V16.24H147.852V10.64H112.212ZM169.18 10.64V40H176.14V18.76L197.98 40H206.18V10.64H199.26V32.88L177.38 10.64H169.18ZM228.256 40V10.8H247.096C260.296 10.8 267.176 15.64 267.176 24.92C267.176 34.84 260.296 40 247.096 40H228.256ZM235.136 16V34.64H247.096C255.376 34.64 259.696 31.36 259.696 25.04C259.696 19.08 255.376 16 247.096 16H235.136Z"
          fill="black"
        />
        <path
          d="M120.6 77H122.06L116.9 62.94H115.42L110.26 77H111.72L113.32 72.52H118.98L120.6 77ZM116.18 64.4C116.18 64.4 116.48 65.56 116.72 66.22L118.56 71.32H113.72L115.6 66.22C115.84 65.56 116.14 64.4 116.14 64.4H116.18ZM130.364 77H131.744V71.48H135.224C137.744 71.48 139.564 69.8 139.564 67.18C139.564 64.58 137.744 62.94 135.224 62.94H130.364V77ZM131.744 70.24V64.18H135.064C136.924 64.18 138.144 65.28 138.144 67.18C138.144 69.1 136.924 70.24 135.044 70.24H131.744ZM148.317 77H149.697V71.48H153.177C155.697 71.48 157.517 69.8 157.517 67.18C157.517 64.58 155.697 62.94 153.177 62.94H148.317V77ZM149.697 70.24V64.18H153.017C154.877 64.18 156.097 65.28 156.097 67.18C156.097 69.1 154.877 70.24 152.997 70.24H149.697Z"
          fill="black"
        />
      </svg>
    </div>

    <!-- Search bar -->
    <b-container>
      <b-form @submit.prevent="fetchData()">
        <b-form-input
          class="mx-auto mt-5 w-50 px-4 my-4"
          type="search"
          placeholder="Research an article"
          v-model="search"
        />
      </b-form>
      <br />
    </b-container>

    <!-- Filters -->

    <b-container>
      <ul class="nav justify-content-center">
        <li class="nav-item">
          <a class="nav-link active" aria-current="page" href="#">Fashion</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Technology</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Environment</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Politics</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Finance</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Society</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Sport</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Culture</a>
        </li>
      </ul>
    </b-container>

    <!-- Articles -->
    <br />
    <b-container>
      <b-row>
        <b-col
          cols="4"
          class="d-flex align-items-stretch"
          v-for="article in articles"
          :key="article.id"
          @click="openLink(article.url)"
        >
          <b-card
            :title="article.title"
            :img-src="article.urlToImage"
            class="mb-5 px-0 shadow-none border-0"
          >
            <b-card-text>{{ article.description }}</b-card-text>
            <b-card-text class="text-muted">{{ article.author }}</b-card-text>
            <b-card-text class="text-muted bottom">{{
              article.publishedAt | formatDate
            }}</b-card-text>
          </b-card>
        </b-col>
      </b-row>
      <b-pagination
        pills
        align="center"
        v-model="currentPage"
        :total-rows="totalResults"
        :per-page="newsPerPage"
        class="mt-5 mb-5"
        @input="updatePage(currentPage)"
      />
    </b-container>
  </div>
</template>

<script>
  import moment from 'moment'

  export default {
    name: 'News',
    props: ['apiKey'],
    data: () => {
      return {
        url: '',
        currentPage: 1,
        totalResults: 0,
        newsPerPage: 10,
        articles: [],
        search: '',
      }
    },
    filters: {
      formatDate: function (value) {
        if (value) {
          return moment(String(value)).format('MM/DD/YYYY hh:mm')
        }
      },
    },
    methods: {
      fetchData() {
        if (this.search !== '') {
          this.url =
            'https://newsapi.org/v2/everything?q=' +
            this.search +
            '&pageSize=' +
            this.newsPerPage +
            '&apiKey=' +
            this.apiKey
        } else {
          this.url =
            'https://newsapi.org/v2/top-headlines?country=de&pageSize=' +
            this.newsPerPage +
            '&apiKey=' +
            this.apiKey

          this.search = ''
        }

        this.articles = []

        let request = new Request(this.url + '&page=' + this.currentPage)
        fetch(request)
          .then((response) => response.json())
          .then((data) => {
            this.totalResults = data.totalResults
            data.articles.forEach((element) => {
              this.articles.push(element)
            })
          })
          .catch((error) => {
            console.log(error)
          })
      },

      updateNewsPerPage(value) {
        this.newsPerPage = value
        this.currentPage = 1
        this.fetchData()
      },

      updatePage(page) {
        this.currentPage = page
        this.fetchData()
      },

      openLink(url) {
        window.open(url)
      },
    },
    created() {
      this.fetchData()
    },
  }
</script>

<style scoped>
  img {
    height: 250px;
    width: 100%;
    object-fit: cover;
  }
  .bottom {
    position: absolute;
    right: 10px;
    bottom: 10px;
  }
  .card {
    border-radius: 4px;
    background: #fff;
    box-shadow: 0 6px 10px rgba(0, 0, 0, 0.08), 0 0 6px rgba(0, 0, 0, 0.05);
    transition: 0.3s transform cubic-bezier(0.155, 1.105, 0.295, 1.12),
      0.3s box-shadow,
      0.3s -webkit-transform cubic-bezier(0.155, 1.105, 0.295, 1.12);
    cursor: pointer;
  }

  .card:hover {
    transform: scale(1.05);
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.12), 0 4px 8px rgba(0, 0, 0, 0.06);
  }
</style>
