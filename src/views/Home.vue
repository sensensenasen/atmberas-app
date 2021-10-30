<template>
  <div class="home">
    <Navbar />
    <div class="container">
      <Hero />
      <div class="row mt-4">
        <div class="col-2">
          <button class="btn btn-md btn-success" @click="onRefresh">Refresh</button>
        </div>
      </div>
      <div class="row mt-2">
        <div class="col" style="height: 300px">
          <ag-grid-vue style="width: 100%; height: 100%" class="ag-theme-alpine" :columnDefs="columnDefs" :rowData="logs" @grid-ready="onGridReady" :overlayLoadingTemplate="overlayLoadingTemplate"> </ag-grid-vue>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from "@/components/Navbar.vue";
import Hero from "@/components/Hero.vue";
import axios from "axios";

import { AgGridVue } from "ag-grid-vue";
import "ag-grid-community/dist/styles/ag-grid.css";
import "ag-grid-community/dist/styles/ag-theme-alpine.css";

import moment from "moment";

export default {
  name: "Home",
  components: {
    Navbar,
    Hero,
    "ag-grid-vue": AgGridVue,
  },
  data() {
    return {
      logs: [],

      //table properti
      gridApi: null,
      columnApi: null,
      columnDefs: null,
      rowData: null,
      overlayLoadingTemplate: '<span class="ag-overlay-loading-center">Refreshing data...</span>',
    };
  },
  beforeMount() {
    var self = this;
    self.columnDefs = [
      { field: "createdAt", headerName: "Date Log", type: ["dateColumn", "nonEditableColumn"] },
      { field: "log_type", headerName: "Log" },
      { field: "uid_card", headerName: "Card" },
      { field: "username", headerName: "Name" },
      { field: "nik", headerName: "NIK" },
      { field: "berat_beras", headerName: "Berat Beras" },
      { field: "counter", headerName: "Penghitung" },
    ];
  },
  methods: {
    onGridReady(params) {
      this.gridApi = params.api;
      this.gridColumnApi = params.columnApi;

      const updateData = (data) => {
        this.setLogs(data);
      };

      fetch("http://159.89.205.119/logs")
        .then((resp) => resp.json())
        .then((data) => updateData(data));
    },
    getData: function() {
      var self = this;

      self.gridApi.showLoadingOverlay();
      axios
        .get("http://159.89.205.119/logs")
        .then((response) => {
          self.setLogs(response.data);
          self.gridApi.hideOverlay();
        })
        .catch((error) => console.log(error));
    },
    onRefresh: function(e) {
      e.preventDefault();
      this.getData();
    },
    setLogs(data) {
      var self = this;
      self.logs = [];
      data.forEach((elem) => {
        var nd = new Date(elem.createdAt);
        var stringDate = moment(nd).format("lll");

        if (elem.log_type == "penerima") {
          self.logs.push({
            uid_card: elem.uid_card,
            username: elem.username,
            nik: elem.nik,
            berat_beras: elem.berat_beras + " Kg",
            log_type: "Ambil Beras",
            counter: elem.counter,
            createdAt: stringDate,
          });
        } else if (elem.log_type == "penyumbang") {
          self.logs.push({
            uid_card: elem.uid_card,
            username: elem.username,
            nik: elem.nik,
            berat_beras: elem.berat_beras + " Kg",
            log_type: "Sumbang Beras",
            counter: elem.counter,
            createdAt: stringDate,
          });
        }
      });
    },
  },
  mounted() {
    //this.getData();
  },
};
</script>
