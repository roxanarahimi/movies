<template>
  <div class="details px-4 px-md-2 ">
  <suspense>
    <template #default >
      <movie-data :id="this.$route.params.id" />
    </template>
    <template #fallback><movie-data-skeleton /></template>
  </suspense>


  </div>
</template>

<script>

import Loader from '../components/Loader';
import MovieData from './MovieData'
import MovieDataSkeleton from './MovieDataSkeleton'

export default {
  components: {Loader, MovieData, MovieDataSkeleton},

}
</script>

<style>

</style>