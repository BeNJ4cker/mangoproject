<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <h1>แก้ไขสไลด์</h1>
        <v-alert v-if="errors" color="danger">
          {{ errors }}
        </v-alert>
      </v-col>
    </v-row>
    <v-divider></v-divider>
    <v-row>
      <v-col cols="6">
        <v-card class="pa-5">
          <v-form @submit="updateSchool()">
            <v-text-field label="ชื่อโรงเรียน" v-model="name"></v-text-field>
            <v-text-field
              label="เรียง"
              type="number"
              v-model="sort"
            ></v-text-field>
            <v-btn type="submit" class="mt-5" color="warning" block
              >แก้ไขโรงเรียน</v-btn
            >
          </v-form>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import axios from "axios";
export default {
  mounted() {
    this.getSchoolInfo();
  },
  data() {
    return {
      name: "",
      sort: "",
      errors: null
    };
  },
  methods: {
    getSchoolInfo() {
      axios
        .get(
          process.env.VUE_APP_MAIN_API +
            "/api/school/" +
            this.$route.params.id
        )
        .then(response => {
          this.name = response.data.data.name;
          this.sort = response.data.data.sort;
        });
    },
    updateSchool() {
      axios
        .put(
          process.env.VUE_APP_MAIN_API +
            "/api/school/" +
            this.$route.params.id,
          {
            name: this.name,
            link: this.link,
            sort: this.sort,
            image: this.image
          }
        )
        .then(response => {
          if (response.data.status == "success") {
            this.$router.push("/admin/school");
          } else {
            this.errors = "มีบางอย่างผิดพลาด";
          }
        });
    }
  }
};
</script>
