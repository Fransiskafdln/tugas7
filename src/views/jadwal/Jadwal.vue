<template>
  <div class="jadwal">
    <!--<img alt="Vue logo" src="../assets/logo.png"> -->
    <Slider />
    <hr class="my-3">
      <router-link class="btn btn-primary" to="/createjadwal">Add Jadwal</router-link>
    <table class="table table-striped">
  <thead>
    <tr>
        <th scope="col">ID</th>
      <th scope="col">Jadwal</th>
      <th scope="col">Nama Matakuliah</th>
      <th scope="col">Aksi</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(jadwals, index) in jadwal" :key="index">
      <td>{{ jadwals.id }}</td>
      <td>{{ jadwals.jadwal }}</td>
      <td>{{ jadwals.matakuliah_id }}</td>
      <td>
        <router-link class="btn btn-success" :to="{name:'Editjadwal', params:{id:jadwals.id}}">Edit</router-link>
        <button @click.prevent="jadwalDelete(jadwals.id)" class="btn btn-danger">Delete</button>
      </td>
    </tr>
  </tbody>
</table>
  </div>
</template>

<script>
import axios from 'axios'
// @ is an alias to /src
// import Slider from "@/components/Slider.vue";
import { ref, onMounted } from 'vue';
export default {
  
  setup(){
    let jadwal = ref([])
    onMounted(() => {
      axios.get('http://127.0.0.1:8000/api/jadwal')
      .then(response => {
        jadwal.value = response.data.data
      })
      .catch(error => {
        console.log(error)
      })
    })
    function jadwalDelete(id){
      axios.delete(`http://127.0.0.1:8000/api/jadwal/${id}`)
      .then(()=>{
        let z = this.jadwal.map(jadwal => jadwal.id).indexOf(id);
        this.jadwal.splice(z, 1)
      }).catch(error => {
        console.log(error)
      })
    }
      return {
      jadwal,
      jadwalDelete
    }
  }
};
</script>