<template>
  <div class="details px-4 px-md-2 ">
    <loader/>
    <div v-if="item.id" class=" container-fluid m-0 p-0 mx-auto">

      <div id="title-box" class="col-12 rounded row mx-0">
        <div class="col-4 col-lg-2 text-end">
          <router-link to="/" id="back-btn" class="btn text-light mt-4 px-4 "><i class="bi bi-arrow-left"></i>
            Back
          </router-link>
        </div>
        <div class="col-8 col-lg-6 ps-5">
          <p class="fw-bold mt-3 mb-0 ">{{ item.title }}</p>
          <p>{{ item.tagline }}</p>
        </div>
      </div>
      <div id="movie-details" class="row justify-content-center  justify-content-lg-start" style="margin-top: 78px">
        <div class="col-sm-10 col-md-8 col-lg-4">
          <img id="poster" :src="'https://image.tmdb.org/t/p/w500'+item.poster_path" class="rounded float-start w-100"
               alt="">
        </div>
        <div class="col-sm-10 col-md-8 ps-lg-5">
          <table class="w-100">
            <tr>
              <th class="text-start">Budget</th>
              <td class="text-end">{{ item.budget }}</td>
            </tr>
            <tr>
              <th class="text-start">Revenue</th>
              <td class="text-end">{{ item.revenue }}</td>
            </tr>
            <tr>
              <th class="text-start">Release Date</th>
              <td class="text-end">{{ item.release_date }}</td>
            </tr>
            <tr>
              <th class="text-start">Runtime</th>
              <td class="text-end">{{ item.runtime }}</td>
            </tr>
            <tr>
              <th class="text-start">Score</th>
              <td class="text-end">{{ item.vote_average + '(' + item.vote_count + ' votes)' }}</td>
            </tr>
            <tr>
              <th class="text-start">Genres</th>
              <td class="text-end"><span v-for="genre in item.genres" :key="genre.id">{{ genre.name + ', ' }}</span>
              </td>
            </tr>
            <tr>
              <th class="text-start">IMDB Link</th>
              <td class="text-end">
                <a v-if="item.imdb_id" class="link" target="_blank" :href="'https://www.imdb.com/title/'+item.imdb_id">Link</a>
              </td>
            </tr>
            <tr>
              <th class="text-start">Homepage Link</th>
              <td class="text-end"><a class="link" v-if="item.homepage" target="_blank" :href="item.homepage">Link</a>
              </td>
            </tr>

          </table>
        </div>
        <div class="col-sm-10 col-md-8 col-lg-12" style="margin-top: 50px; font-size: 16px">
          <p>{{ item.overview }}</p>
          <p class="fw-bold" style="margin-top: 110px; font-size: 18px"> Credit:</p>
          <p id="cast" style=" font-size: 14px; margin-bottom:100px"></p>
        </div>
      </div>

    </div>
    <p class="text-black-50" id="msg"></p>

  </div>
</template>

<script>
import {onMounted, ref} from 'vue';
import router from '@/router';
import Loader from '../components/Loader'

export default {
  components: {Loader},
  setup() {
    const id = ref(null);
    id.value = router.currentRoute.value.params.id;
    const item = ref({});
    const getItem = () => {
      document.querySelector('#loader').classList.remove('d-none');
      axios.get('https://api.themoviedb.org/3/movie/' + id.value + '?api_key=f62f750b70a8ef11dad44670cfb6aa57&language=en-US')
          .then((response) => {
            item.value = response.data;
            console.log(response.data)
            document.querySelector('#loader').classList.add('d-none');
          })
          .catch((error) => {
            if (error.response.status === 404) {
              document.querySelector('#loader').classList.add('d-none');
              document.querySelector('#msg').innerText = 'Movie was not found.'
            } else {
              document.querySelector('#msg').innerText = error.message;
            }
            console.log(error.status)
          });
    };
    const getItemCast = () => {
      axios.get('https://api.themoviedb.org/3/movie/' + id.value + '/credits?api_key=f62f750b70a8ef11dad44670cfb6aa57&language=en-US')
          .then((response) => {
            let castStr = '';
            let cast = response.data.cast;

            cast = cast.sort((a, b) => {
              return b.popularity - a.popularity
            })
            for (let i = 0; i <= 9; i++) {
              if (cast[i]) {
                castStr = castStr + cast[i].name + ', ';
              }
            }
            document.getElementById('cast').innerText = castStr;
          })
          .catch((error) => {
            console.log(error)
          });
    };

    onMounted(() => {
      getItem();
      getItemCast();
      setTimeout(() => {
        document.querySelector('#loader').classList.add('d-none');
      }, 5000);

    });
    return {
      item, getItem, getItemCast
    }
  }

}
</script>

<style>
#movie-details p {
  text-align: justify;
}

@media (min-width: 1024px) {
  #poster {
    width: 330px;
  }
}

#title-box {
  background-color: #e2e2e2;
  width: 100%;
  min-height: 85px;
  text-align: left !important;
  font-size: 18px;
}

#back-btn {
  border-radius: 50px;
  background-color: #549df2;
}

th, td {
  height: 40px;
  vertical-align: middle;
  font-size: 16px
}
</style>