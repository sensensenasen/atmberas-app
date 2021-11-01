<template>
  <div class="hero">
    <!-- Desktop -->
    <div class="d-none d-md-block">
      <div class="row mt-4">
        <div class="col-md-6">
          <div class="d-flex h-100">
            <div class="justify-content-center align-self-center">
              <h2>
                Setup
                <strong>Kartu</strong>
                <br />ATM Beras
              </h2>
              <p>Daftarkan user dan kartu baru di sini!</p>
              <!-- <router-link class="btn btn-lg btn-success" to="/foods"> <b-icon-arrow-right></b-icon-arrow-right>Pesan </router-link> -->
            </div>
          </div>
        </div>
        <div class="col-md-6">
          <img src="../assets/images/hero4.png" width="80%" />
        </div>
      </div>
    </div>

    <!-- Mobile -->
    <div class="d-sm-block d-md-none">
      <div class="row mt-4">
        <div class="col-md-6 mb-3">
          <img src="../assets/images/hero4.png" width="80%" />
        </div>
        <div class="col-md-6">
          <div class="d-flex h-100">
            <div class="justify-content-center align-self-center">
              <h2>
                Setup
                <strong>Kartu</strong>
                <br />ATM Beras
              </h2>
              <p>Daftarkan user dan kartu baru di sini!</p>
              <!-- <button class="btn btn-lg btn-success"><b-icon-arrow-right></b-icon-arrow-right>Pesan</button> -->
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "BannerSetup",
};
</script>

<style></style>
