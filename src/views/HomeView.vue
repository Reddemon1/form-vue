<template>
  <div class="home">

  </div>
</template>

<script>

import axios from 'axios'
export default {
  name: 'Home',
  data() {
    return {
      siswa: [],
    }
  },
  methods:{
    getSiswa(){
      axios.get('http://localhost:8000/api/siswa')
      .then((response) =>{
        console.log(response);
      })
    }
  },
  created() {
    this.getSiswa();
  },
}
</script>
