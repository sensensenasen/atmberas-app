<template>
  <div>
    <Navbar />
    <div class="container">
      <BannerSetup />
      <div class="row mt-4">
        <div class="col">
          <button class="btn btn-md btn-success" @click="$bvModal.show('bv-modal-example')">Add User</button>
        </div>
      </div>
      <div class="row mt-2">
        <div class="col" style="height: 300px;">
          <ag-grid-vue style="width: 100%; height: 100%;" class="ag-theme-alpine" :columnDefs="columnDefs" :rowData="users" @grid-ready="onGridReady" :overlayLoadingTemplate="overlayLoadingTemplate"> </ag-grid-vue>
        </div>
      </div>
      <div class="row mt-4 justify-content-center">
        <div class="col-3">
          <button class="btn btn-sm btn-danger" @click="onReset">Reset Penghitung</button>
        </div>
      </div>
    </div>

    <!-- Modal add user -->
    <b-modal id="bv-modal-example" hide-footer>
      <template #modal-title> Add User </template>
      <AddUser @close-modal="eventCallback" />
    </b-modal>
  </div>
</template>

<script>
// @ is an alias to /src
// import axios from "axios";
import Navbar from "@/components/Navbar.vue";
import BannerSetup from "@/components/BannerSetup.vue";
import AddUser from "@/components/AddUser.vue";

import axios from "axios";

import { AgGridVue } from "ag-grid-vue";
import "ag-grid-community/dist/styles/ag-grid.css";
import "ag-grid-community/dist/styles/ag-theme-alpine.css";
import moment from "moment";

export default {
  name: "Setup",
  components: {
    Navbar,
    BannerSetup,
    "ag-grid-vue": AgGridVue,
    AddUser,
  },
  data() {
    return {
      users: [],

      //table properti
      gridApi: null,
      columnApi: null,
      overlayLoadingTemplate: '<span class="ag-overlay-loading-center">Refreshing data...</span>',
      columnDefs: null,
      rowData: null,
    };
  },
  beforeMount() {
    var self = this;
    self.columnDefs = [
      { field: "uid_card", headerName: "Card" },
      { field: "username", headerName: "Name" },
      { field: "nik", headerName: "NIK" },
      { field: "role", headerName: "Role" },
      { field: "counter", headerName: "Penghitung" },
      { field: "createdAt", headerName: "Created At", type: ["dateColumn", "nonEditableColumn"] },
      { field: "updatedAt", headerName: "Updated At", type: ["dateColumn", "nonEditableColumn"] },
    ];
  },
  methods: {
    onGridReady(params) {
      this.gridApi = params.api;
      this.gridColumnApi = params.columnApi;

      const updateData = (data) => {
        this.setUsers(data);
      };

      fetch("http://159.89.205.119/users")
        .then((resp) => resp.json())
        .then((data) => updateData(data));
    },
    getData: function() {
      var self = this;

      self.gridApi.showLoadingOverlay();
      axios
        .get("http://159.89.205.119/users")
        .then((response) => {
          self.setUsers(response.data);
          self.gridApi.hideOverlay();
        })
        .catch((error) => console.log(error));
    },
    setUsers(data) {
      var self = this;
      self.users = [];
      data.forEach((elem) => {
        var nd = new Date(elem.createdAt);
        var stringDate1 = moment(nd).format("lll");
        var nd2 = new Date(elem.updatedAt);
        var stringDate2 = moment(nd2).format("lll");

        self.users.push({
          uid_card: elem.uid_card,
          username: elem.username,
          nik: elem.nik,
          log_type: elem.log_type,
          role: elem.role,
          counter: elem.counter,
          createdAt: stringDate1,
          updatedAt: stringDate2,
        });
      });
      //this.logs = data;
    },
    onReset: function(e) {
      var self = this;
      e.preventDefault();
      axios
        .get("http://159.89.205.119/reset")
        .then((response) => {
          if (response.data.message) {
            self.$toast.success(response.data.message, {
              type: "success",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            });
            self.getData();
          } else if (response.data.error) {
            self.$toast.info(response.data.error, {
              type: "info",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            });
          }
        })
        .catch((error) => console.log(error));
    },
    eventCallback: function() {
      this.$bvModal.hide("bv-modal-example");
      this.getData();
    },
  },
  mounted() {
    //this.getData();
  },
};
</script>
<style></style>
