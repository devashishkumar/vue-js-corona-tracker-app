<template>
  <div class="container">
    <span v-if="loading">Data is preparing</span>
    <div class="data-table" v-if="!loading">
      <table class="table table-hover">
        <thead>
          <tr>
            <th>City</th>
            <th>Active</th>
            <th>Confirmed</th>
            <th>Deceased</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in coronaData" :key="item.city">
            <td>{{item.city}}</td>
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
    HelloWorld
  },
  data() {
    return {
      loading: false,
      urlParams: this.$route.params.id,
      coronaData: []
    };
  },
  created() {
    this.loading = true;
    fetch(DATA_URL)
      .then(response => {
        return response.json();
      })
      .then(data => {
        this.manipulateServiceData(data);
        this.loading = false;
      })
      .catch(error => {
        console.log(error);
        this.loading = false;
      });
  },
  methods: {
    manipulateServiceData: function(data: any) {
        const citiesData: any = [];
        Object.keys(data[this.urlParams]['districtData']).forEach(e => {
            const currentData = data[this.urlParams]['districtData'][e];
            currentData['city'] = e;
            citiesData.push(currentData);
        });
        this.coronaData = citiesData;
    }
  }
})
export default class State extends Vue {}
</script>
