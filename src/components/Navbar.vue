<template>
  <div>
    <a href="#">Home</a>
    <Upload />
  </div>
</template>

<script>
import Upload from "./Upload";

export default {
  name: "Navbar",
  components: {
    Upload,
  },
  data: () => ({
    funky: "Home",
  }),
};
</script>

<style></style>
