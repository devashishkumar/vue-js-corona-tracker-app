<template>
  <div class="container">
    <form>
  <div class="form-group">
    <label for="exampleInputEmail1">Enter Your Name</label>
    <input type="name" class="form-control" id="name" placeholder="Enter Your Name">
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import HelloWorld from "@/components/HelloWorld.vue"; // @ is an alias to /src

@Options({
  components: {
    HelloWorld
  },
  methods: {
    manipulateServiceData: function (data: any) {
        console.log('test');
    },
  },
})
export default class Login extends Vue {}
</script>
