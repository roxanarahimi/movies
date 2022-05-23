<template>
  <div class="home row px-4 px-md-2 m-0">
    <div id="search-box" class="col-12 rounded row mx-0">
      <div class="col-md-4 col-lg-3 text-lg-end">
        <label for="search" class="mt-4 ">Search by release date </label>
      </div>
      <div class="col-12 col-md-5 col-lg-7">
        <date id="search" class="border mt-3" style="max-width: 350px;"/>
      </div>
      <div class="col-6 col-md-3  col-lg-2  text-sm-end text-lg-start">
        <button id="search-btn" @click="search" class="btn text-light mb-3 mb-lg-0  mt-4">Search</button>
      </div>
    </div>
    <loader/>
    <div v-if="movies.length" id="movies"
         class="col-12 row d-flex justify-content-sm-center justify-content-md-between justify-content-lg-start px-md-4 px-lg-0 mx-0">
      <div v-for="item in movies" :key="item.id"
           class="movie px-lg-3 mb-4 pb-2 col-sm-8 col-md-6 col-lg-4 d-flex justify-content-center">
        <router-link :to="'/movie/'+ item.id">
          <div class="card">
            <div class="card-body">
              <div class="row">
                <div class="col-6">
                  <img :src="'https://image.tmdb.org/t/p/w500'+item.poster_path" class="img-fluid" alt=""/>
                </div>
                <div class="col-6 py-2 pe-4 ps-0 " style="display: grid;">

                  <div style="align-self: start">
                    <p class="fw-bold" style="font-size: 16px">{{ item.title }}</p>
                  </div>
                  <div style="font-size: 12px; align-self: end">
                    <p>
                      <i class="bi bi-calendar"></i>
                      {{ item.release_date }}
                    </p>

                    <span v-for="(genre, index) in item.genres" :key="index" class="me-1">
                      <i :class="{'d-none' : index===0}" style="font-size: 4px"
                         class="bi bi-circle-fill ms-1"></i> {{ genre }}</span>
                  </div>

                </div>
              </div>
            </div>
          </div>
        </router-link>
      </div>
      <div class="paginate text-center">
        <nav>
          <span @click="prevPage()" :class="{'text-muted': (currentPage <= 1 ),  'fw-bold pointer': currentPage > 1}">Previuos Page</span><br
            class="d-md-none">
          |
          <span :class="{'text-primary': item == currentPage}" :id="'page_'+item "
                v-for="item in total_pages_array" :key="item" @click="goToPage(item)"
                class="pointer fs-6 fw-bold page_number mx-3">{{ item }}</span>| <br class="d-md-none">
          <span @click="nextPage()"
                :class="{'text-muted': currentPage >= total_pages, 'fw-bold pointer': currentPage < total_pages}">Next Page</span>
          <p class="text-black-50 mt-2">Showing results 1-20</p>
        </nav>
      </div>
    </div>
    <p class="text-black-50" id="msg"></p>

  </div>
</template>

<script>
import {onMounted, ref} from 'vue';
import moment from 'moment';
import Date from '../components/Date';
import Loader from '../components/Loader';

export default {
  components: {Date, Loader},
  name: "Home",
  setup() {
    const movies = ref([]);
    const allMovies = ref([]);
    const currentPage = ref(1);
    const total_pages = ref(1);
    const total_pages_array = ref([1]);
    const genres_array = ref([])

    const getGenres = () => {
      axios.get('https://api.themoviedb.org/3/genre/movie/list?api_key=f62f750b70a8ef11dad44670cfb6aa57&language=en-US')
          .then((response) => {
            response.data.genres.forEach((element) => {
              genres_array[element.id] = element.name;
            });
            total_pages_array.value = [];
            for (let i = 1; i <= total_pages.value; i++) {
              if (i <= total_pages.value) {
                total_pages_array.value.push(i);
              }
            }
          }).catch((error) => {
        console.error(error)
      });
    };
    const getMovies = () => {
      document.querySelector('#loader').classList.add('d-none');
      axios.get('https://api.themoviedb.org/4/list/1?page=' + currentPage.value + '&api_key=f62f750b70a8ef11dad44670cfb6aa57')
          .then((response) => {
            movies.value = response.data.results;

            // get all movies
            allMovies.value = [];
            let obj_ids = response.data.object_ids;
            let keys = Object.keys(obj_ids);
            keys.forEach((key) => {
              let id = key.replace('movie:', '');
              axios.get('https://api.themoviedb.org/3/movie/' + id + '?api_key=f62f750b70a8ef11dad44670cfb6aa57&language=en-US')
                  .then((response) => {
                    let m = response.data;
                    let mgs = m.genres;
                    let object1 = [];
                    mgs.forEach((g) => {
                      object1.push(g.name);
                    })
                    m.genres = object1;
                    allMovies.value.push(m);
                  }).catch();
            });
            if (movies.value.length > 0) {
              document.querySelector('#loader').classList.add('d-none');
            } else {
              document.querySelector('#msg').innerText = 'No movies found';
            }
            total_pages.value = response.data.total_pages;
            movies.value.forEach((movie) => {
              let gs = [];
              movie.genre_ids.forEach((id) => {
                gs.push(genres_array[id]);
              });
              movie.genres = gs;
            })
            total_pages_array.value = [];
            for (let i = 1; i <= total_pages.value; i++) {
              if (i <= total_pages.value) {
                total_pages_array.value.push(i);
              }
            }

            document.querySelector('.paginate')?.classList.remove('d-none');
          })
          .catch((error) => {
            console.error(error);
            document.querySelector('#msg').innerText = error.message;
          })
    };
    const goToPage = (i) => {
      if (0 < i <= total_pages.value) {
        currentPage.value = i;
        getMovies();
      }
    };
    const prevPage = () => {
      if (currentPage.value > 1) {
        currentPage.value -= 1;
        getMovies();
      }
    };
    const nextPage = () => {
      if (currentPage.value < total_pages.value) {
        currentPage.value += 1;
        getMovies();
      }
    };
    const search = () => {
      document.querySelector('.dp__clear_icon')?.addEventListener('click', () => {
        getMovies();
        console.log('listens');
      })
      let range = document.querySelector('.dp__input').value.replace(' - ', '#');
      range = range.replaceAll('/', '-');
      let rangeArray = range.split('#');
      if (rangeArray.length < 2 || rangeArray[1] == '') {
        document.querySelector('.dp__input').value = '';
        getMovies();
        alert('you have tho select two dates');
      }
      let all = [];
      all = allMovies.value;
      document.querySelector('.paginate')?.classList.add('d-none');
      movies.value = all.filter(movie => (moment(rangeArray[0]) <= moment(movie.release_date) && moment(movie.release_date) <= moment(rangeArray[1]))
      );
    }
    onMounted(() => {
      getGenres();
      getMovies();
      document.querySelector('#loader').classList.remove('d-none');
      setTimeout(() => {
        document.querySelector('#loader').classList.add('d-none');
      }, 5000);

    });
    return {
      movies, currentPage, total_pages, total_pages_array, getMovies, goToPage, prevPage, nextPage, getGenres, search
    }
  }
};
</script>

<style scoped>
#movies {
  margin-top: 120px;
}

.card {
  border-color: #c4c4c4;
  background-color: #f1f1f1;
  border-radius: 6px;
  padding: 0;
}
@media (min-width: 1200px) {
  .movie .card {
    max-width: 295px;
  }
}
.card-body {
  padding: 3px;
}

.card-body p {
  text-align: left !important;
  margin-bottom: 10px;
}

.card img {
  border-top-left-radius: 6px;
  border-bottom-left-radius: 6px;
}

#search-box {
  background-color: #e2e2e2;
  width: 100%;
  min-height: 85px;
}

#search-btn {
  border-radius: 50px;
  background-color: #549df2;
}
</style>