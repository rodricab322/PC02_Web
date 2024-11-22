<template>
  <q-page>
    <q-header class="bg-primary text-white">
      <q-input
        filled
        v-model="searchQuery"
        placeholder="Search"
        @keyup.enter="searchMovies"
        class="q-my-md"
      />
    </q-header>

    <q-page-container>
      <q-page>
        <div class="q-gutter-md row justify-center">
          <div v-for="movie in movies" :key="movie.id" class="movie">
            <img :src="IMG_URL + movie.poster_path" :alt="movie.title" />
            <div class="movie-info">
              <h3>{{ movie.title }}</h3>
              <span :class="getColor(movie.vote_average)">
                {{ movie.vote_average }}
              </span>
            </div>
            <div class="overview">
              <h3>Overview</h3>
              <p>{{ movie.overview }}</p>
            </div>
          </div>
        </div>
      </q-page>
    </q-page-container>
  </q-page>
</template>



<style scoped>
:root {
  --primary-color: #3d436f;
  --secondary-color: #272e41;
}

.movie {
  width: 300px;
  margin: 1rem;
  border-radius: 3px;
  box-shadow: 0.2px 4px 5px rgba(0, 0, 0, 0.1);
  background-color: var(--secondary-color);
  position: relative;
  overflow: hidden;
}
.movie img {
  width: 100%;
}
.movie-info {
  color: #1c1920;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.5rem 1rem 1rem;
  letter-spacing: 0.5px;
}
.movie-info h3 {
  margin-top: 0;
}
.movie-info span {
  background-color: var(--primary-color);
  padding: 0.25rem 0.5rem;
  border-radius: 3px;
  font-weight: bold;
}
.movie-info span.green {
  color: lightgreen;
}
.movie-info span.orange {
  color: orange;
}
.movie-info span.red {
  color: red;
}
.overview {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #322525;
  padding: 1rem;
  max-height: 100%;
  transform: translateY(101%);
  transition: transform 0.3s ease-in;
}
.movie:hover .overview {
  transform: translateY(0);
}
</style>

<script>
export default {
  data() {
    return {
      API_KEY: "df4d3777b4423ccc8de83cf8a18d7a1f",
      BASE_URL: "https://api.themoviedb.org/3",
      API_URL: "",
      IMG_URL: "https://image.tmdb.org/t/p/w500",
      movies: [],
      searchQuery: "",
    };
  },
  mounted() {
    this.API_URL =
      this.BASE_URL + "/discover/movie?sort_by=popularity.desc&api_key=" + this.API_KEY;
    this.getMovies(this.API_URL);
  },
  methods: {
    async getMovies(url) {
      try {
        const res = await fetch(url);
        const data = await res.json();
        this.movies = data.results;
      } catch (error) {
        console.error("Error fetching movies:", error);
      }
    },
    searchMovies() {
      if (this.searchQuery) {
        const searchURL =
          `${this.BASE_URL}/search/movie?api_key=${this.API_KEY}&query=` +
          this.searchQuery;
        this.getMovies(searchURL);
      } else {
        this.getMovies(this.API_URL);
      }
    },
    getColor(vote) {
      if (vote >= 8) return "green";
      else if (vote >= 5) return "orange";
      else return "red";
    },
  },
};
</script>
