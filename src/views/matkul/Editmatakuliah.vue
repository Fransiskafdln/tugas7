<template>
  <div class="card shadow mt-3">
    <div class="card-body">
      <h5 class="card-title">Edit Matakuliah</h5>
      <form class="row g-3" @submit.prevent="update">
        <div class="col-md-6">
          <label for="inputEmail4" class="form-label">Nama Matakuliah</label>
          <input
            type="text"
            class="form-control"
            id="inputEmail4"
            v-model="matakuliahs.nama_matakuliah"
          />
          <div class="alert alert-danger" v-if="validation.nama_matakuliah">
            {{ validation.nama_matakuliah[0] }}
          </div>
        </div>
        <div class="col-md-6">
          <label for="inputEmail4" class="form-label">SKS</label>
          <input
            type="text"
            class="form-control"
            id="inputEmail4"
            v-model="matakuliahs.sks"
          />
          <div class="alert alert-danger" v-if="validation.sks">
            {{ validation.sks[0] }}
          </div>
        </div>

        <div class="col-12">
          <button type="submit" class="btn btn-primary">Edit</button>
        </div>
      </form>
    </div>
  </div>
</template>
<script>
import { onMounted, ref } from "vue";
import { reactive } from "vue";
import { useRouter, useRoute } from "vue-router";
import axios from "axios";
export default {
  setup() {
    const matakuliahs = reactive({
      nama: "",
      sks: "",
    });
    const validation = ref([]);
    const router = useRouter();
    const route = useRoute();
    onMounted(() => {
      axios
        .get(`http://127.0.0.1:8000/api/matkul/${route.params.id}`)
        .then((response) => {
          console.log(response.data.data.nama_matakuliah);
          matakuliahs.nama_matakuliah = response.data.data.nama_matakuliah;
          matakuliahs.sks = response.data.data.sks;
        })
        .catch((error) => {
          console.log(error.response.data);
        });
    });
    function update() {
      let nama_matakuliah = matakuliahs.nama_matakuliah;
      let sks = matakuliahs.sks;
      axios
        .put(`http://127.0.0.1:8000/api/matkul/${route.params.id}`, {
          nama_matakuliah: nama_matakuliah,
          sks: sks,
        })
        .then(() => {
          router.push({
            name: "Index",
          });
        })
        .catch((error) => {
          console.log(error);
        });
    }
    return {
      matakuliahs,
      validation,
      router,
      update,
      route,
    };
  },
};
</script>