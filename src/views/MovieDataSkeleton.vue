<template>
  <suspense>
    <div class=" container-fluid  mx-0 px-0">

      <div id="title-boxx" class="col-12 rounded row mx-0 element">
        <div class="col-4 col-lg-2 text-end">

        </div>
        <div class="col-8 col-lg-6 ps-5">
          <!--          &lt;!&ndash;        title&ndash;&gt;-->
          <!--          <p class="fw-bold mt-3 mb-0 element"></p>-->
          <!--          &lt;!&ndash;        tagline&ndash;&gt;-->
          <!--          <p class="element"></p>-->
        </div>
      </div>
      <div id="movie-details" class="row  container-fluid mx-0 px-5 px-lg-0  " style="margin-top: 78px">
        <div id="poster" class="col-sm-12 col-md-4 col-lg-4 element mx-auto ju">

        </div>
        <div class="col-sm-12 col-md-8 ps-lg-5  text-center mx-auto">
          <table class="w-100 px-lg-5  mt-5 mt-lg-0">
            <tr class="w-100">
              <!--              <th class="text-start">Budget</th>-->
              <!--              budget-->
              <th class="text-end pe-lg-5"><p class="element "></p></th>
              <td class=""></td>
              <td class="text-end ps-lg-5"><p class="element  ms-lg-5"></p></td>

            </tr>
            <tr>
              <!--              <th class="text-start">Revenue</th>-->
              <!--              Revenue-->
              <th class="text-end pe-5"><p class="element "></p></th>
              <td class=""></td>
              <td class="text-end ps-5"><p class="element  ms-5"></p></td>
            </tr>
            <tr>
              <!--              <th class="text-start">Release Date</th>-->
              <!--              Release Date-->
              <th class="text-end pe-5"><p class="element "></p></th>
              <td class=""></td>
              <td class="text-end ps-5"><p class="element  ms-5"></p></td>
            </tr>
            <tr>
              <!--              <th class="text-start">Runtime</th>-->
              <!--              Runtime-->
              <th class="text-end pe-5"><p class="element "></p></th>
              <td class=""></td>
              <td class="text-end ps-5"><p class="element  ms-5"></p></td>
            </tr>
            <tr>
              <!--              <th class="text-start">Score</th>-->
              <!--              Score-->
              <th class="text-end pe-5"><p class="element "></p></th>
              <td class=""></td>
              <td class="text-end ps-5"><p class="element  ms-5"></p></td>
            </tr>
            <tr>
              <!--              <th class="text-start">Genres</th>-->
              <!--              Genres-->
              <th class="text-end pe-5"><p class="element "></p></th>
              <td class=""></td>
              <td class="text-end ps-5"><p class="element  ms-5"></p></td>
            </tr>
            <tr>
              <!--              <th class="text-start">IMDB Link</th>-->
              <!--              IMDB Link-->
              <th class="text-end pe-5"><p class="element "></p></th>
              <td class=""></td>
              <td class="text-end ps-5"><p class="element  ms-5"></p></td>
            </tr>
            <tr>
              <!--              <th class="text-start">Homepage Link</th>-->
              <!--              Homepage Link-->
              <th class="text-end pe-5"><p class="element "></p></th>
              <td class=""></td>
              <td class="text-end ps-5"><p class="element  ms-5"></p></td>
            </tr>

          </table>
        </div>
        <div class="col-sm-10 col-md-12   container-fluid mx-0 px-0" style="margin-top: 50px; font-size: 16px">
          <!--          overview-->
          <p class="element"></p>
          <p class="element"></p>
          <p class="element w-50"></p>
          <!--          Credit-->
          <p class="fw-bold element mb-3 w-25" style="margin-top: 90px; font-size: 18px"></p>
          <p class="element" id="cast"></p>
        </div>
      </div>

    </div>
  </suspense>
</template>
<script>
import {ref} from 'vue';
import Loader from '../components/Loader'

export default {}
</script>
<style scoped>
#movie-details p {
  min-height: 15px;
  /*width: 300px;*/
}

@keyframes pulse-bg {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0.5;
  }
  100% {
    opacity: 1;
  }
}

.element {
  background-color: #eaeaea;
  border-radius: 3px;
  animation: pulse-bg 1s infinite;

}

@media (min-width: 1024px) {
  #poster {
    width: 320px;
    height: 480px;
  }
}

/*@media (max-width: 1024px) {*/
/*  #poster {*/
/*    width: 612px;*/
/*    height: 918px;*/
/*  }*/
/*}*/

@media (max-width: 1024px) {
  #poster {
    /*width: calc(100% - 10rem);*/
    /*height: calc((100% - 10rem)* 3 / 2);*/
    width: 625px;
    height: 938px;
  }
}

@media (max-width: 756px) {
  #poster {
    width: 499px;
    height: 749px;
  }
}


#title-boxx {
  /*background-color: #e2e2e2;*/
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
  font-size: 16px;
  padding: 5px 0;
}

p {
  margin-bottom: 10px;
}
</style>