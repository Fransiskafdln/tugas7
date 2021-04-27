<template>
  <div class="card shadow mt-3">
    <div class="card-body">
      <h5 class="card-title">Edit Jadwal</h5>
      <form class="row g-3" @submit.prevent="update">
        <div class="col-md-6">
          <label for="inputEmail4" class="form-label">Jadwal</label>
          <input
            type="text"
            class="form-control"
            id="inputEmail4"
            v-model="jadwals.jadwal"
          />
          <div class="alert alert-danger" v-if="validation.jadwal">
            {{ validation.jadwal[0] }}
          </div>
        </div>
        <div class="col-md-6">
          <label for="inputEmail4" class="form-label">Nama Matakuliah</label>
          <input
            type="text"
            class="form-control"
            id="inputEmail4"
            v-model="jadwal.matakuliah_id"
          />
          <div class="alert alert-danger" v-if="validation.matakuliah_id">
            {{ validation.matakuliah_id[0] }}
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
    const jadwals = reactive({
      jadwal: "",
      matakuliah_id: "",
    });
    const validation = ref([]);
    const router = useRouter();
    const route = useRoute();
    onMounted(() => {
      axios
        .get(`http://127.0.0.1:8000/api/jadwal/${route.params.id}`)
        .then((response) => {
          console.log(response.data.data.jadwal);
          jadwal.jadwal = response.data.data.jadwal;
          jadwal.matakuliah_id = response.data.data.matakuliah_id;
        })
        .catch((error) => {
          console.log(error.response.data);
        });
    });
    function update() {
      let jadwal = jadwal.jadwal;
      let matakuliah_id = jadwals.matakuliah_id;
      axios
        .put(`http://127.0.0.1:8000/api/jadwal/${route.params.id}`, {
          jadwal: jadwal,
          matakuliah_id: matakuliah_id,
        })
        .then(() => {
          router.push({
            name: "Jadwal",
          });
        })
        .catch((error) => {
          console.log(error);
        });
    }
    return {
      jadwal,
      validation,
      router,
      update,
      route,
    };
  },
};
</script>