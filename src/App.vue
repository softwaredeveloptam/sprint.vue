<template>
  <div id="app">
    <Navbar v-bind:currentView="currentView" v-on:change-view="changeView" />
    <!-- pass urls to AllPhotos component -->
    <div class="photo-gallery" v-if="currentView === 'AllPhotos'">
      <div v-for="photo in this.urls" :key="photo.urlKey">
        <AllPhotos v-bind:photo="photo.url" v-bind:currentView="currentView" v-on:send-photo="selectPhoto"/>
      </div>
    </div>
    <SinglePhoto v-else v-bind:selectedPhoto="selectedPhoto"/>
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
    SinglePhoto
  },
  data: () => ({
    currentView: "AllPhotos",
    //an array of images represented as base-64 strings
    urls: [],
    //an image represented as a base-64 string
    selectedPhoto: ""
  }),
  async created() {
    await this.fetchPhotos();
  },
  methods: {
    fetchPhotos: async function() {
      const allPhotoObjects = await listObjects();
      const photoKeys = await Promise.all(allPhotoObjects.map(e => e.Key));
      const baseStringsArray = await Promise.all(
        photoKeys.map(photo => getSingleObject(photo))
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
    selectPhoto: function(photo) {
      this.selectedPhoto = photo;
      this.currentView = "singlePhoto";
    },
        changeView: function() {
      this.currentView = "AllPhotos";
    },
  }
};
</script>

<style>
#app {
  text-align: center;
}

.photo-gallery {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
}
</style>