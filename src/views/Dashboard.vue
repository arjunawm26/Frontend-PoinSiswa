<template>
  <div class="dashboard">
    <!-- partial -->
    <div class="container-fluid page-body-wrapper">
      <div class="main-panel">
        <div class="content-wrapper">
          <div class="row">
            <div class="col-md-6 col-lg-6 grid-margin stretch-card">
              <div class="row">
                <div class="col-md-6 col-lg-6 grid-margin stretch-card">
                  <div class="card bg-gradient-primary text-white text-center card-shadow-primary">
                    <div class="card-body">
                      <h6 class="font-weight-normal">Data Siswa</h6>
                      <h2 class="mb-0">{{ data_siswa }}</h2>
                    </div>
                  </div>
                </div>
                <div class="col-md-6 col-lg-6 grid-margin stretch-card">
                  <div class="card bg-gradient-warning text-white text-center card-shadow-warning">
                    <div class="card-body">
                      <h6 class="font-weight-normal">Data Petugas</h6>
                      <h2 class="mb-0">{{ data_petugas }}</h2>
                    </div>
                  </div>
                </div>
                <div class="col-md-6 col-lg-6 grid-margin stretch-card">
                  <div class="card bg-gradient-danger text-white text-center card-shadow-danger">
                    <div class="card-body">
                      <h6 class="font-weight-normal">Data Pelanggaran</h6>
                      <h2 class="mb-0">{{ data_pelanggaran }}</h2>
                    </div>
                  </div>
                </div>
                <div class="col-md-6 col-lg-6 grid-margin stretch-card">
                  <div class="card bg-gradient-success text-white text-center card-shadow-success">
                    <div class="card-body">
                      <h6 class="font-weight-normal">Siswa Melanggar Hari Ini</h6>
                      <h2 class="mb-0">{{ siswa_melanggar }}</h2>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="col-md-6 col-lg-6 grid-margin stretch-card">
              <div class="card">
                <div class="card-body">
                  <div class="d-flex justify-content-between">
                    <h6 class="card-title"><i class="mdi mdi-finance menu-icon"></i> Poin Tertinggi</h6>
                  </div>
                  <div v-for="item in poin_tertinggi" v-bind:key="item.nama_siswa" class="list d-flex align-items-center border-bottom pb-3">
                    <div class="wrapper w-100">
                      <p><b>{{item.nama_siswa}}</b> <span class="badge badge-danger">{{item.total_poin}}</span></p>
                      <small class="text-muted">{{item.kelas}}</small>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- partial -->
      </div>
      <!-- main-panel ends -->
    </div>
    <!-- page-body-wrapper ends -->
  </div>
  <!-- container-scroller -->
</template>

<script>
module.exports = {
  data(){
    return{
      data_siswa: 0,
      data_petugas: 0,
      data_pelanggaran: 0,
      siswa_melanggar: 0,
      poin_tertinggi: [],
    }
  },
  methods: {
    getData(){
      let conf = { headers: { "Authorization" : 'Bearer ' + this.key } };
      let offset = (this.currentPage - 1) * this.perPage;
      this.$bvToast.show("loadingToast");
      this.axios.get("/beranda/statistik", conf)
      .then(response => {
        this.data_siswa       = response.data.jml_siswa;
        this.data_petugas     = response.data.jml_petugas;
        this.data_pelanggaran = response.data.jml_data_pelanggaran;
        this.siswa_melanggar  = response.data.pelanggaran_hari_ini;
        console.log(response.data)
      });
      this.axios.get("/beranda/poin_tertinggi", conf)
      .then(response => {
        this.poin_tertinggi = response.data.data;
        console.log(response.data)
      })
    }
  },
  mounted(){
    this.key = localStorage.getItem("Authorization");
    this.getData();
  }
}
</script>
