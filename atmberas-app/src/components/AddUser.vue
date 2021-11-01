<template>
  <div class="container-fluid">
    <div class="col">
      <b-form @submit="onSubmit" v-if="show">
        <b-form-group id="input-group-1" label="Nama:" label-for="input-1">
          <b-form-input id="input-1" v-model="form.username" placeholder="Masukan Nama" required></b-form-input>
        </b-form-group>

        <b-form-group id="input-group-2" label="UID Kartu:" label-for="input-2">
          <b-form-input id="input-2" v-model="form.uid_card" placeholder="Masukan UID Kartu" required></b-form-input>
        </b-form-group>

        <b-form-group id="input-group-2" label="NIK:" label-for="input-3">
          <b-form-input id="input-2" v-model="form.nik" placeholder="Masukan NIK" required></b-form-input>
        </b-form-group>

        <b-form-group id="input-group-3" label="Role:" label-for="input-4">
          <b-form-select id="input-3" v-model="form.role" :options="roles" required></b-form-select>
        </b-form-group>
        <div class="row mb-2 justify-content-center">
          <div class="col-12">
            <b-button type="submit" variant="success">Submit</b-button>
          </div>
        </div>
      </b-form>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

import axios from "axios";

export default {
  name: "AddUser",
  components: {},
  data() {
    return {
      form: {
        username: "",
        uid_card: "",
        nik: "",
        role: null,
      },
      roles: [{ text: "Select One", value: null }, "penerima", "penyumbang"],
      show: true,
    };
  },
  methods: {
    onSubmit(event) {
      var self = this;
      event.preventDefault();
      var submitObj = {
        username: self.form.username,
        uid_card: self.form.uid_card,
        nik: self.form.nik,
        role: self.form.role,
        counter: 0,
      };

      axios
        .post("http://159.89.205.119/users", submitObj)
        .then((response) => {
          if (response.data) {
            self.$toast.success("SUCCESS", {
              type: "success",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            });
            self.resetForm();
            self.$emit("close-modal");
          }
        })
        .catch((error) => {
          console.log(error);
          self.$toast.info("CARD ALREADY REGISTERED", {
            type: "info",
            position: "top-right",
            duration: 3000,
            dismissible: true,
          });
        });
    },
    resetForm() {
      this.form.username = "";
      this.form.uid_card = "";
      this.form.nik = "";
      this.form.role = null;
      // Trick to reset/clear native browser form validation state
      this.show = false;
    },
  },
};
</script>
<style></style>
