<template>
  <!-- <img alt="Vue logo" src="../assets/logo.png" />
    <HelloWorld msg="Welcome to Your Vue.js + TypeScript App" /> -->
  <div class="container">
    <div class="data-table">
      <table class="table table-hover">
        <thead>
          <tr>
            <th>State</th>
            <th>Active</th>
            <th>Confirmed</th>
            <th>Deceased</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in coronaData" :key="item.state">
            <td><router-link :to="'/state/' + item.state">{{item.state}}</router-link></td>
            <td>{{item.active}}</td>
            <td>{{item.confirmed}}</td>
            <td>{{item.deceased}}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import HelloWorld from "@/components/HelloWorld.vue"; // @ is an alias to /src
const DATA_URL = "https://api.covid19india.org/state_district_wise.json";

@Options({
  components: {
    HelloWorld,
  },
  data() {
    return {
      coronaData: [],
    };
  },
  created() {
    fetch(DATA_URL)
      .then((response) => {
        return response.json();
      })
      .then((data) => {
        console.log(data, "36");
        this.manipulateServiceData(data);
      })
      .catch((error) => {
        console.log(error);
      });
  },
  methods: {
    manipulateServiceData: function (data: any) {
      // alert(serviceData)
      const sData: any = [];
      if (data && Object.keys(data).length > 0) {
        Object.keys(data).forEach((s) => {
          if (s !== "State Unassigned") {
            if (data[s]["districtData"]) {
              const districtData = {
                state: s,
                active: 0,
                deceased: 0,
                confirmed: 0,
              };
              Object.keys(data[s]["districtData"]).forEach((dd) => {
                districtData.active =
                  districtData.active + data[s]["districtData"][dd]["active"];
                districtData.deceased =
                  districtData.deceased +
                  data[s]["districtData"][dd]["deceased"];
                districtData.confirmed =
                  districtData.confirmed +
                  data[s]["districtData"][dd]["confirmed"];
              });
              sData.push(districtData);
            }
          }
        });
      }
      this.coronaData = sData;
    },
  },
})
export default class Home extends Vue {}
</script>
