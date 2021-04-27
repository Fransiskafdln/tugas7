<template>
  <div class="card shadow mt-3">
    <div class="card-body">
      <h5 class="card-title">Add Semester</h5>
      <form class="row g-3" @submit.prevent="store">
        <div class="col-md-6">
          <label for="inputEmail4" class="form-label">Semester</label>
          <input
            type="text"
            class="form-control"
            id="inputEmail4"
            v-model="semesters.semester"
          />
          <div class="alert alert-danger" v-if="validation.semester">
            {{ validation.semester[0] }}
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
import { ref } from "vue";
import { reactive } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";
export default {
  setup() {
    const semesters = reactive({
      semester: "",
    });
    const validation = ref([]);
    const router = useRouter();
    function store() {
      let semester = semesters.semester;
      axios
        .post("http://127.0.0.1:8000/api/semester", {
          semester: semester,
        })
        .then(() => {
          router.push({
            name: "Smt",
          });
        })
        .catch((error) => {
          console.log(error);
        });
    }
    return {
      semesters,
      validation,
      router,
      store,
    };
  },
};
</script>