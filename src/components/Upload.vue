<template>
      <button @click="myClickEvent" >
        <input ref="input" type="file" class="hide" accept="image/gif, image/jpeg, image/png" v-on:change="uploadFile"></input>
      Upload</button>
</template>

<script>
import { saveObject } from "../../utils/index.js";


export default {
  name: "Upload",
  data: () => ({
    upload: "Upload",
  }),
  methods: {
    myClickEvent($event) {
      const input = this.$refs.input
      input.click();
    },
    uploadFile(element) {
      saveObject(element.target.files[0]);
      alert("Upload Success!");
    }
  }
};
</script>

<style scoped>
.hide {
  display: none;
}
</style>

