<template>
  <div>
    <h1>This is All Photos Component</h1>
    <p v-bind:urls="urls" v-for="url in urls">{{ url }}</p>
  </div>
</template>

<script>
export default {
  name: "AllPhotos",
  props: ["urls"],
  /*
      photoOutput = baseStringsArray.map(photo => {
        return (
          <img src={`data:image/jpg;base64, ${photo}`} className="allPhotos" />
        );
      });
      }
    */
};
</script>

<style></style>
