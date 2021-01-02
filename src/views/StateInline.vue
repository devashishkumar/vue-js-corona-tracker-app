<template>
  <div class="container">
    <span v-if="loading">Data is preparing</span>
    <span style="display: block;">We are showing data for the state {{ urlParams }}</span>
    <span @click="displayStatesData()">Back To States</span>
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
          <tr v-for="item in coronaData" :key="item.state">
            <td>{{ item.city }}</td>
            <td>{{ item.active }}</td>
            <td>{{ item.confirmed }}</td>
            <td>{{ item.deceased }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";

@Options({
  components: {
  },
  props: {
    urlParams: "",
    data: {}
  },
  data() {
    return {
      loading: false,
      coronaData: []
    };
  },
  created() {
    // window.console.log(this.data, this.urlParams, "52");
    this.manipulateServiceData(this.data);
    // this.scrollTopWindow();
  },
  methods: {
    scrollTopWindow: function() {
      window.scroll(0, 0);
    },
    displayStatesData: function () {
        this.$emit('openStates', {currentParam: this.urlParams});
    },
    manipulateServiceData: function (data: any) {
      const citiesData: any = [];
      // window.console.log(this.data, this.urlParams, "52");
      Object.keys(data[this.urlParams]["districtData"]).forEach(e => {
        const currentData = data[this.urlParams]["districtData"][e];
        currentData["city"] = e;
        citiesData.push(currentData);
      });
      this.coronaData = citiesData;
    }
  }
})
export default class StateInline extends Vue {}
</script>
