<template>
  <div class="card shadow mt-3">
    <div class="card-body">
      <h5 class="card-title">Add Absen</h5>
      <form class="row g-3" @submit.prevent="store">
        <div class="col-md-6">
          <label for="inputEmail4" class="form-label">Waktu absen</label>
          <input
            type="text"
            class="form-control"
            id="inputEmail4"
            v-model="absen.waktu_absen"
          />
          <div class="alert alert-ganger" v-if="validation.waktu_absen">
            {{ validation.waktu_absen[0] }}
          </div>
        </div>
        <div class="col-md-6">
          <label for="inputPassword4" class="form-label">Mahasiswa ID</label>
          <input
            type="number"
            class="form-control"
            id="inputPassword4"
            v-model="absen.mahasiswa_id"
          />
          <div class="alert alert-ganger" v-if="validation.mahasiswa_id">
            {{ validation.mahasiswa_id[0] }}
          </div>
        </div>
        <div class="col-md-6">
          <label for="inputPassword4" class="form-label">Matakuliah ID</label>
          <input
            type="number"
            class="form-control"
            id="inputPassword4"
            v-model="absen.matakuliah_id"
          />
          <div class="alert alert-ganger" v-if="validation.matakuliah_id">
            {{ validation.matakuliah_id[0] }}
          </div>
        </div>
        <div class="col-md-6">
          <label for="inputPassword4" class="form-label">Keterangan</label>
          <input
            type="number"
            class="form-control"
            id="inputPassword4"
            v-model="absen.keterangan"
          />
          <div class="alert alert-ganger" v-if="validation.keterangan">
            {{ validation.keterangan[0] }}
          </div>
        </div>
        <div class="col-12">
          <button type="submit" class="btn btn-primary">Add</button>
        </div>
      </form>
    </div>
  </div>
</template>
<script>
import { reactive, ref } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";
export default {
  setup() {
    const absen = reactive({
      waktu_absen: "",
       mahasiswa_id: "",
      matakuliah_id: "",
       keterangan: "",
    });
    const validation = ref([]);
    const router = useRouter();
    function store() {
      let waktu_absen = absen.waktu_absen;
      let mahasiswa_id = absen.mahasiswa_id;
      let matakuliah_id = absen.matakuliah_id;
      let keterangan = absen.keterangan;
      axios
        .post('http://127.0.0.1:8000/api/absen', {
          waktu_absen: waktu_absen,
          mahasiswa_id: mahasiswa_id,
          matakuliah_id: matakuliah_id,
          keterangan: keterangan,
        })
        .then(() => {
          router.push({
            name: "Home",
          });
        })
        .catch((error) => {
          console.log(error);
        });
    }
    return {
      absen,
      validation,
      router,
      store,
    };
  },
};
</script>