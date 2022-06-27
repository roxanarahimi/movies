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
        <button id="search-btn" @click="getMovies" class="btn text-light mb-3 mb-lg-0  mt-4">Search</button>
      </div>
    </div>
    <div v-if="movies.length" id="movies"
         class="col-12 row d-flex justify-content-sm-center justify-content-md-between justify-content-lg-start px-md-4 px-lg-0 mx-0">
      <div v-for="item in movies" :key="item.id"
           class="movie px-lg-3 mb-4 pb-2 col-sm-8 col-md-6 col-lg-4 d-flex justify-content-center">
        <suspense>

          <template #default>
            <movie-card :id="item.id"/>
          </template>
          <template #fallback>
            <loader/>
          </template>
        </suspense>
      </div>
      <pagination  :currentPage="currentPage" :total_pages="total_pages" :total_pages_array="total_pages_array" :getMovies="getMovies" />
    </div>

    <p class="text-black-50" id="msg"></p>

  </div>
</template>

<script>
import {onMounted, ref} from 'vue';
import moment from 'moment';
import Date from '../components/Date';
import Loader from '../components/Loader';
import MovieCard from '../components/MovieCard';
import Pagination from '../components/Pagination';

export default {
  components: {
    Date,
    Loader,
    MovieCard,
    Pagination
  },
  name: "Home",
  setup() {
    const movies = ref([]);
    // const allMovies = ref([]);
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
    const getMovies = (p) => {
      console.log(p)
      if (p === undefined) {
        currentPage.value = 1;
      } else if (1 <= p && p <= total_pages.value) {
        currentPage.value = p;
      }else{
        currentPage.value = 1;
      }
      movies.value = [];
      let range = document.querySelector('.dp__input').value;
      range = range.replace(' - ', '#');
      range = range.replaceAll('/', '-');
      let rangeArray = range.split('#');
      console.log(rangeArray)
      let start = rangeArray[0];
      let end = rangeArray[1];
      if (rangeArray.length < 2 || rangeArray[1] == '') {
        document.querySelector('.dp__input').value = '';
        start='';
        end='';
        // alert('you have tho select two dates');
      }
      console.log('&primary_release_date.gte='+start+'&primary_release_date.lte='+end)
      axios.get('https://api.themoviedb.org/3/discover/movie?page=' + currentPage.value + '&primary_release_date.gte='+start+'&primary_release_date.lte='+end+'&api_key=f62f750b70a8ef11dad44670cfb6aa57')
      // https://api.themoviedb.org/3/discover/movie?api_key=<<api_key>>&language=en-US&sort_by=popularity.desc&include_adult=false&include_video=false&page=1&with_watch_monetization_types=flatrate
          .then((response) => {
            movies.value = response.data.results;
            // allMovies.value = [];
            // let obj_ids = response.data.object_ids;
            // let keys = Object.keys(obj_ids);
            // keys.forEach((key) => {
            //   let id = key.replace('movie:', '');
            //   axios.get('https://api.themoviedb.org/3/movie/' + id + '?api_key=f62f750b70a8ef11dad44670cfb6aa57&language=en-US')
            //       .then((response) => {
            //         let m = response.data;
            //         let mgs = m.genres;
            //         let object1 = [];
            //         mgs.forEach((g) => {
            //           object1.push(g.name);
            //         })
            //         m.genres = object1;
            //         allMovies.value.push(m);
            //       }).catch();
            // });
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

            setTimeout(() => {
              document.querySelector('.paginate')?.classList.remove('d-none');
            }, 500);
          })
          .catch((error) => {
            console.error(error);
            document.querySelector('#msg').innerText = error.message;
          })
    };
    const search = () => {
      getMovies();
      // document.querySelector('.dp__clear_icon')?.addEventListener('click', () => {
      //   // movies.value = [];
      //   getMovies();
      //   console.log('listens');
      // })
      // let range = document.querySelector('.dp__input').value.replace(' - ', '#');
      // range = range.replaceAll('/', '-');
      // let rangeArray = range.split('#');
      // if (rangeArray.length < 2 || rangeArray[1] == '') {
      //   document.querySelector('.dp__input').value = '';
      //   getMovies();
      //   alert('you have tho select two dates');
      // }
      // let all = [];
      // all = allMovies.value;
      // movies.value = all.filter(movie => (moment(rangeArray[0]) <= moment(movie.release_date) && moment(movie.release_date) <= moment(rangeArray[1])));
      // setTimeout(() => {
      //   document.querySelector('.paginate')?.classList.add('d-none');
      // }, 100);
      // setTimeout(() => {
      // }, 1000);

    }
    onMounted(() => {
      getGenres();
      getMovies();
    });
    return {
      movies, currentPage, total_pages, total_pages_array, getMovies,  getGenres, search
    }
  }
};
</script>

<style scoped>
#movies {
  margin-top: 120px;
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