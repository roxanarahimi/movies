<template>
  <suspense>
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
                         class="bi bi-circle-fill ms-1"></i> {{ genre.name }}</span>
              </div>

            </div>
          </div>
        </div>
      </div>
    </router-link>
  </suspense>
</template>
<script>
import Loader from "@/components/Loader";
import {ref} from "vue";

export default {
  name: "MovieCard",
  props: ['id'],
  components: {Loader},
  async setup(props) {
    const item = ref({});
    await axios.get('https://api.themoviedb.org/3/movie/' + props.id + '?api_key=f62f750b70a8ef11dad44670cfb6aa57&language=en-US')
        .then((response) => {
          item.value = response.data;
        })
        .catch((error) => {
          console.error(error)
        })

    return {
      item
    }
  }
}
</script>

<style scoped>

.card {
  border-color: #c4c4c4;
  background-color: #f1f1f1;
  border-radius: 6px;
  padding: 0;
}

@media (min-width: 1200px) {
  .movie .card {
    width: 295px !important;
    height: 205px !important;
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
</style>