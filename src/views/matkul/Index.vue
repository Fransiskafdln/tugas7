<template>
  <div class="home">
    <!--<img alt="Vue logo" src="../assets/logo.png"> -->
    <Slider />
    <hr class="my-3" />
    <router-link class="btn btn-primary" to="/creatematakuliah"
      >Add Matakuliah</router-link
    >
    <table class="table table-striped">
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Nama Matakuliah</th>
          <th scope="col">SKS</th>
          <th scope="col">Aksi</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(data, index) in matkul" :key="index">
          <td>{{ data.id }}</td>
          <td>{{ data.nama_matakuliah }}</td>
          <td>{{ data.sks }}</td>
          <td>
            <router-link
              class="btn btn-success"
              :to="{ name: 'Editmatakuliah', params: { id: data.id } }"
              >Edit</router-link
            >
            <button
              @click.prevent="matakuliahDelete(data.id)"
              class="btn btn-danger"
            >
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
// @ is an alias to /src
// import Slider from "@/components/Slider.vue";
import { ref, onMounted } from "vue";
export default {
  setup() {
    let matkul = ref([]);
    onMounted(() => {
      axios
        .get("http://127.0.0.1:8000/api/matkul")
        .then((response) => {
          matkul.value = response.data.data;
        })
        .catch((error) => {
          console.log(error);
        });
    });
    function matakuliahDelete(id) {
      axios
        .delete(`http://127.0.0.1:8000/api/matkul/${id}`)
        .then(() => {
          let z = this.matkul.map((matkul) => matkul.id).indexOf(id);
          this.matkul.splice(z, 1);
        })
        .catch((error) => {
          console.log(error);
        });
    }
    return {
      matkul,
      matakuliahDelete,
    };
  },
};
</script>