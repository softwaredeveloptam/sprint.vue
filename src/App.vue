<template>
  <div id="app">
    <Navbar v-bind:currentView="currentView" />
    <!-- pass urls to AllPhotos component -->
    <AllPhotos v-bind:urls="urls" v-if="currentView === 'AllPhotos'" />
    <SinglePhoto v-else />
    <div v-for="photo in this.urls" :key="photo.urlKey" src="photo">
      <allphotos :photo="photo" v-on:getphoto="getphoto" />
    </div>

    {{ urls }}
  </div>
</template>

<script>
import Navbar from "./components/Navbar";
import AllPhotos from "./components/AllPhotos";
import SinglePhoto from "./components/SinglePhoto";
import { listObjects, getSingleObject } from "../utils/index.js";

export default {
  name: "App",
  components: {
    Navbar,
    AllPhotos,
    SinglePhoto,
  },
  data: () => ({
    currentView: "SinglePhoto",
    //an array of images represented as base-64 strings
    urls: [],
    //an image represented as a base-64 string
    selectedPhoto: "",
  }),
  async created() {
    await this.fetchPhotos();
  },
  methods: {
    fetchPhotos: async function() {
      const allPhotoObjects = await listObjects();
      const photoKeys = await Promise.all(allPhotoObjects.map((e) => e.Key));
      const baseStringsArray = await Promise.all(
        photoKeys.map((photo) => getSingleObject(photo))
      );

      const urls = await Promise.all(
        baseStringsArray.map((string, index) => {
          let newObj = {};

          newObj["url"] = `data:image/jpg;base64, ${string}`;
          newObj["urlKey"] = index;

          return newObj;
        })
      );

      this.urls = urls;
    },
  },
};
</script>

<style>
#app {
  text-align: center;
}
</style>

<!-- 

 async function photoFetcher() {
    const allPhotoObjects = await listObjects();

    const photoKeys = await Promise.all(allPhotoObjects.map(e => e.Key));
    const baseStringsArray = await Promise.all(
      photoKeys.map(photo => getSingleObject(photo))
    );

    setPhotos(baseStringsArray);
    return baseStringsArray;
  }

  const photoOutput = baseStringsArray.map(photo => {
    return (
      <img src={`data:image/jpg;base64, ${photo}`} className="allPhotos" />
    );
  });

-->
