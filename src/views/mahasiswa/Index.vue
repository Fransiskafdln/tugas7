<template>
  <div class="mahasiswas">
    <Slider />
    <hr class="my-3" />
    <router-link class="btn btn-primary" to="/createmahasiswas"
      >Add Mahasiswa</router-link
    >
    <table class="table">
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Nama Mahasiswa</th>
          <th scope="col">Alamat</th>
          <th scope="col">No Tlp</th>
          <th scope="col">Email</th>
          <th scope="col">Aksi</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(data, index) in mahasiswas" :key="index">
          <td>{{ mahasiswa.nama_mahasiswa }}</td>
          <td>{{ mahasiswa.alamat }}</td>
          <td>{{ mahasiswa.no_tlp }}</td>
          <td>{{ mahasiswa.email }}</td>
          <td>
            <router-link
              class="btn btn-success"
              :to="{ name: 'Editmahasiswas', params: { id: mahasiswa.id } }"
              >Edit</router-link
            >
            <button
              @click.prevent="mahasiswaDelete(mahasiswa.id)"
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
import { ref, onMounted } from "vue";
export default {
 
  setup() {
    let mahasiswa = ref([]);
    onMounted(() => {
      axios
        .get("http://127.0.0.1:8001/api/mahasiswa")
        .then((response) => {
          mahasiswa.value = response.data.data;
        })
        .catch((error) => {
          console.log(error);
        });
    });
    function mahasiswaDelete(id) {
      axios
        .delete(`http://127.0.0.1:8001/api/mahasiswa/${id}`)
        .then(() => {
          let z = this.mahasiswa
            .map((mahasiswa) => mahasiswa.id)
            .indexOf(id);
          this.mahasiswa.splice(z, 1);
        })
        .catch((error) => {
          console.log(error);
        });
    }
    return {
      mahasiswa,
      mahasiswaDelete,
    };
  },
};
</script>