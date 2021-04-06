<template>
  <div>
    <div class="header">
      <div class="header-navigation">
        <button @click="onClickHome">Home</button>
        <button @click="onClickPrev">Prev</button>
        <button @click="onClickNext">Next</button>
        <button class="button-noaction">Page: {{ currentPage }}</button>
      </div>
      <form @submit.prevent="onSubmitHandler()">
        <input
          class="header-search"
          type="search"
          placeholder="Search"
          ref="searchElement"
          v-model="searchEl"
        />
      </form>
    </div>
    <div class="movie-container">
      <Movie v-for="movie in movies" :key="movie" v-bind="movie" />
    </div>
  </div>
</template>

<script>
import { onMounted, ref } from "vue";
import Movie from "./components/Movie.vue";

const FEATURED_API =
  "https://api.themoviedb.org/3/discover/movie?sort_by=popularity.desc&vote_count.gte=1500&page=";
const SEARCH_API =
  "https://api.themoviedb.org/3/search/movie?&query=";

export default {
  name: "movies-app-vue",
  components: {
    Movie,
  },
  setup() {
    const movies = ref([1, 2, 3]);
    const searchEl = ref("");
    const searchElement = ref(null);
    const currentPage = ref(1);

    onMounted(async () => {
      movies.value = await fetchData(FEATURED_API);
      // console.log(movies.value)
    });
    const fetchData = async (API) => {
      const response = await fetch(API);
      const data = await response.json();
      return data.results;
    };
    const onSubmitHandler = async () => {
      const searchTerm = searchElement.value.value;
      if (searchTerm) {
        movies.value = await fetchData(SEARCH_API + searchTerm);
        searchElement.value.value = "";
        currentPage.value = 1;
      }
    };
    const navigation = async (page) => {
      movies.value = await fetchData(FEATURED_API + page);
    };
    const onClickHome = () => {
      currentPage.value = 1;
      navigation(currentPage.value);
    };
    const onClickPrev = () => {
      if (currentPage.value > 1) {
        currentPage.value -= 1;
        console.log("currentPage", currentPage);
        navigation(currentPage.value);
      }
    };
    const onClickNext = () => {
      currentPage.value += 1;
      console.log("currentPage.value", currentPage.value);
      navigation(currentPage.value);
    };
    return {
      movies,
      onSubmitHandler,
      searchEl,
      searchElement,
      onClickHome,
      onClickPrev,
      onClickNext,
      currentPage,
    };
  },
};
</script>

<style>
.movie-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
.header {
  padding: 1rem;
  background-color: orange;
  display: flex;
  justify-content: space-between;
}
.header-search {
  font-size: 1.2rem;
  border-radius: 50px;
  padding: 0.3rem;
  outline: none;
}
.header-navigation {
  display: flex;
}
.header-navigation button {
  border-radius: 5px;
  outline: none;
  background-color: orangered;
  color: white;
  margin-right: 0.3rem;
  font-weight: bold;
  font-size: 1rem;
}
.header-navigation button:hover {
/* .header-navigation-action { */
  transform: scale(1.1);
  cursor: pointer;
}
.header-navigation .button-noaction:hover {
  transform: scale(1);
  cursor: inherit;
}
</style>
