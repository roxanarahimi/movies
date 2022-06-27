<template>
  <div class="paginate text-center">
    <nav class="px-0">
      <span @click="prevPage()" :class="{'text-muted': (currentPage <= 1 ),  'fw-bold pointer': currentPage > 1}">Previuos Page </span>
      <br class="d-sm-none">|
      <span :class="{'text-primary': currentPage === 1 }" :id="'page_'+1 " @click="goToPage(1)" class="pointer fs-6 fw-bold page_number mx-3">{{ 1 }}</span>
      <span :class="{'text-primary': currentPage === 2 }" :id="'page_'+2 " @click="goToPage(2)" class="pointer fs-6 fw-bold page_number mx-3">{{ 2 }}</span>
      <span :class="{'text-primary': currentPage === 3 }" :id="'page_'+3 " @click="goToPage(3)" class="pointer fs-6 fw-bold page_number mx-3">{{ 3 }}</span>
      <span :class="{'text-primary': currentPage === 4 }" :id="'page_'+4 " @click="goToPage(4)" class="pointer fs-6 fw-bold page_number mx-3">{{ 4 }}</span>
      <span :class="{'text-primary': currentPage === 5 }" :id="'page_'+5 " @click="goToPage(5)" class="pointer fs-6 fw-bold page_number mx-3">{{ 5 }}</span>

      <span v-if="total_pages>5">   ...</span>

      <span v-if="currentPage>5 && currentPage < 500" :class="{'text-primary': currentPage > 5 &&  currentPage < 500}" :id="'page_'+currentPage
       " @click.prevent="goToPage(currentPage)" class="pointer fs-6 fw-bold page_number mx-3">{{ currentPage }}</span>
      <span v-if="currentPage>5 && currentPage < 500"> ... </span>

<!-- this is because of this error that i received for more than 500 pages: 'page must be less than or equal to 500'-->
      <span v-if="total_pages>5 && total_pages<500"
            :class="{'text-primary': currentPage === total_pages}"
            :id="'page_'+total_pages "
            @click.prevent="goToPage(total_pages)"
            class="pointer fs-6 fw-bold page_number mx-3"> {{ total_pages }} </span>

      <span v-if="total_pages>499"
            :class="{'text-primary': currentPage === 500}"
            :id="'page_'+500 "
            @click="goToPage(500)"
            class="pointer fs-6 fw-bold page_number mx-3"> {{ 500 }} </span>|
      <br class="d-sm-none">
      <span @click.prevent="nextPage()"
            :class="{'text-muted': currentPage >= total_pages, 'fw-bold pointer': currentPage < total_pages}">Next Page</span>

      <div class="d-flex justify-content-between">
        <p class="text-black-50 d-inline mt-3 pt-sm-2  ">Showing results {{ currentPage * 20 - 19 }} - {{ currentPage * 20 }}</p>
        <form v-if="total_pages>5" class="mt-3 ms-3 d-inline">
          <label class="d-inline mb-2 me-1" for="pageNum" >go to page </label>
          <input id="pageNum" required class="form-control d-inline m-1" type="number" min="1" max="500" style="width: 80px">
          <input type="submit" class="btn btn-sm btn-primary pt-1 pb-2 mx-1 d-inline" @click.prevent="goToPageNum" value="go" />
          <p id="hint" class="error" style="color: indianred"></p>

        </form>
       </div>
      </nav>

  </div>
</template>
<script>

export default {
  props: ['currentPage', 'total_pages', 'total_pages_array','getMovies'],
  setup(props) {
    const goToPage = (i) => {
      console.log(i)
      if (0 < i <= props.total_pages) {
        // props.currentPage.value = i;
        getMovies(i);
      }
    };
    const prevPage = () => {
      if (props.currentPage > 1) {
        getMovies(props.currentPage -1);
      }
    };
    const nextPage = () => {
      if (props.currentPage < props.total_pages) {
        getMovies(props.currentPage + 1);
      }
    };
    const getMovies = (x) => {
      props.getMovies(x);
    };
    const goToPageNum = ()=>{
      let i = parseInt(document.querySelector('#pageNum').value);
      if (i>0 && i<501){
        document.querySelector('#hint').innerText = '';
        goToPage(i);
      }else{
        document.querySelector('#pageNum').style.border = '2px solid indianred'
        document.querySelector('#hint').innerText = 'page number must be between 1 & 500';
      }
    }
    return {
      goToPage, prevPage, nextPage, getMovies, goToPageNum
    }
  }
}
</script>
<style>
.paginate {
  width: 100% !important;
}
</style>