<template>
  <div class="home">
    <img alt="Covid19 logo" src="../assets/logo-covid19.jpg" />
    <h5>Terakhir Update : {{ lastUpdate }}</h5>
    <br />
    <b-container class="bv-example-row">
      <b-form-select v-model="selected" class="mb-3 form-select">
        <b-form-select-option :value="null">Global</b-form-select-option>
        <b-form-select-option
          v-for="(item, index) in country"
          :value="item.iso3"
          :key="index"
          >{{ item.name }}</b-form-select-option
        > </b-form-select
      ><br />
      <b-row>
        <b-col cols="12" lg="4">
          <div class="card border-primary text-primary">
            <div class="card-header text-primary">KASUS</div>
            <div class="card-body text-primary">
              <h5>{{ confirmed }} Orang</h5>
              <p>Jumlah angka kasus covid-19</p>
            </div>
          </div>
        </b-col>
        <b-col cols="12" lg="4">
          <div class="card border-success text-success">
            <div class="card-header text-success">SEMBUH</div>
            <div class="card-body text-success">
              <h5>{{ recovered }} Orang</h5>
              <p>Jumlah angka sembuh covid-19</p>
            </div>
          </div></b-col
        >
        <b-col cols="12" lg="4">
          <div class="card border-danger text-danger">
            <div class="card-header text-danger">MENINGGAL</div>
            <div class="card-body text-danger">
              <h5>{{ deaths }} Orang</h5>
              <p>Jumlah angka meninggal covid-19</p>
            </div>
          </div></b-col
        > </b-row
      ><br /><br />
      <h5>Created By : Muhammad Zakie</h5>
      <h5>Pada : Ahad, 9 Mei 2021 (27 Ramadhan 1442 H)</h5>
      <p class="text-success">@muhammadzakie22 @programmercintasunnah</p>
    </b-container>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";
export default {
  name: "Home",
  data() {
    return {
      selected: null,
      country: [],
      lastUpdate: "03/05/2021",
      confirmed: 0,
      recovered: 0,
      deaths: 0,
    };
  },
  created() {
    console.log("created works!!!");
    this.getCountry();
    const formatter = new Intl.NumberFormat("en-US", {
      minimumFractionDigits: 0,
    });
    axios
      .get("https://covid19.mathdro.id/api/")
      .then((res) => {
        console.log(res.data);
        this.confirmed = formatter.format(res.data.confirmed.value);
        this.recovered = formatter.format(res.data.recovered.value);
        this.deaths = formatter.format(res.data.deaths.value);
        this.lastUpdate = moment(res.data.lastUpdate).format(
          "YYYY-MM-DD HH:MM:SS"
        );
      })
      .catch((err) => {
        console.log(err);
      });
  },
  watch: {
    selected: function (val) {
      //console.log(val);
      const formatter = new Intl.NumberFormat("en-US", {
        minimumFractionDigits: 0,
      });
      if (val === null) {
        axios
          .get("https://covid19.mathdro.id/api/")
          .then((res) => {
            console.log(res.data);
            this.confirmed = formatter.format(res.data.confirmed.value);
            this.recovered = formatter.format(res.data.recovered.value);
            this.deaths = formatter.format(res.data.deaths.value);
          })
          .catch((err) => {
            console.log(err);
          });
      }
      axios
        .get("https://covid19.mathdro.id/api/countries/" + val)
        .then((res) => {
          console.log(res.data);
          this.confirmed = formatter.format(res.data.confirmed.value);
          this.recovered = formatter.format(res.data.recovered.value);
          this.deaths = formatter.format(res.data.deaths.value);
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  methods: {
    getCountry() {
      axios
        .get("https://covid19.mathdro.id/api/countries")
        .then((res) => {
          console.log(res.data.countries);
          this.country = res.data.countries;
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  getDataCovid() {},
};
</script>

<style scoped>
img {
  width: 400px;
}
</style>
