<template>
  <div>
    <Datepicker v-model="date" range type="date" format="yyyy/MM/dd"/>
  </div>
</template>

<script>
import {ref, onMounted} from 'vue';
import Datepicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css'

export default {
  components: {
    Datepicker
  },
  setup() {
    const date = ref();
    onMounted(() => {
    })
    return {
      date,
    };
  }
}
</script>

<style scoped>

</style>