<template>
  <suspense>
  <div class=" container-fluid m-0 p-0 mx-auto">

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

  <p id="cast" style=" font-size: 14px; margin-bottom:100px">{{ castStr }}</p>
      </div>
    </div>

  </div>
  </suspense>
</template>
<script>
import { ref} from 'vue';
import Loader from '../components/Loader'

export default {
  components: {Loader},
  props: ['id'],
  async setup(props) {
    const item = ref({});
    const castStr = ref('');
    await axios.get('https://api.themoviedb.org/3/movie/' + props.id + '?api_key=f62f750b70a8ef11dad44670cfb6aa57&language=en-US')
          .then(async (response) => {
            item.value =await response.data;

           await axios.get('https://api.themoviedb.org/3/movie/' + props.id + '/credits?api_key=f62f750b70a8ef11dad44670cfb6aa57&language=en-US')
                .then((response) => {
                  castStr.value = '';
                  let cast = response.data.cast;
                  cast = cast.sort((a, b) => {
                    return b.popularity - a.popularity
                  })
                  for (let i = 0; i <= 9; i++) {
                    if (cast[i]) {
                      castStr.value = castStr.value + cast[i].name + ', ';
                    }
                  }
                  console.log('cast', castStr);
                  // document.getElementById('cast').innerText = castStr.value;
                })
                .catch((error) => {
                  console.log(error)
                });
          })
          .catch((error) => {
            if (error.response.status === 404) {
              document.querySelector('#msg').innerText = 'Movie was not found.'
            } else {
              document.querySelector('#msg').innerText = error.message;
            }
            console.log(error.status)
          });



    return {
      item, castStr
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