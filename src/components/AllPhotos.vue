<template>
  <img :src="photo" @click="$emit('send-photo', photo)"/>

</template>

<script>
export default {
  name: "AllPhotos",
  props: ["photo"],
};
</script>

<style scoped>
img {
  width: 95%;
}
</style>


