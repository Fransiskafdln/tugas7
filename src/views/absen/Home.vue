<template>
  <div class="home">
    <Slider />
    <hr class="my-3" />
    <router-link class="btn btn-primary" to="/createabsens"
      >Add absen</router-link
    >
    <table class="table">
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Waktu Absen</th>
          <th scope="col">Mahasiswa ID</th>
          <th scope="col">Matakuliah ID</th>
          <th scope="col">Keterangan</th>
          <th scope="col">Aksi</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(absen, index) in absens" :key="index">
          <td>{{ absen.waktu_absen }}</td>
          <td>{{ absen.mahasiswa_id }}</td>
          <td>{{ absen.matakuliah_id }}</td>
          <td>{{ absen.Keterangan }}</td>
          <td>
            <router-link
              class="btn btn-success"
              :to="{ name: 'Editabsens', params: { id: absen.id } }"
              >Edit</router-link
            >
            <button
              @click.prevent="absenDelete(absen.id)"
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
import Slider from "@/components/Slider.vue";
import { ref, onMounted } from "vue";
export default {
  name: "Home",
  components: {
    Slider,
  },
  setup() {
    let absens = ref([]);
    onMounted(() => {
      axios
        .get("http://127.0.0.1:8000/api/absen")
        .then((response) => {
          absens.value = response.data.data;
        })
        .catch((error) => {
          console.log(error);
        });
    });
    function absenDelete(id) {
      axios
        .delete(`http://127.0.0.1:8000/api/absen/${id}`)
        .then(() => {
          let z = this.absen.map((absen) => absens.id).indexOf(id);
          this.absen.splice(z, 1);
        })
        .catch((error) => {
          console.log(error);
        });
    }
    return {
      absen,
      absenDelete,
    };
  },
};
</script>