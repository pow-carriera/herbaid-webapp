<script setup>
import { ref, onUpdated, watch, onMounted } from "vue";
import axios from "axios";
const show = ref(false);
const props = defineProps(["id", "name", "content"]);
const carousel = ref();

const carouselload = ref(true);
onUpdated(() => {
  axios
    .get(
      "https://herbalaid-server.onrender.com/api/remedies/" +
        props.id +
        "?populate=carousel"
    )
    .then((response) => {
      carousel.value = response.data.data.attributes.carousel.data;
      console.log(carousel.value);
      carouselload.value = false;
    });
});

onMounted(()=> {
  axios
    .get(
      "https://herbalaid-server.onrender.com/api/remedies/" +
        props.id +
        "?populate=carousel"
    )
    .then((response) => {
      carousel.value = response.data.data.attributes.carousel.data;
      console.log(carousel.value);
      carouselload.value = false;
    });
})
</script>

<template>
  <v-card class="mx-auto my-5" max-width="1100" elevation="4">
    <v-card-title>
      <h3 class="my-2 mx-auto text-center">{{ name }}</h3>
    </v-card-title>

    <div v-if="carouselload" class="d-flex justify-center my-5 mt-16">
      <v-progress-circular indeterminate :color="'green'"></v-progress-circular>
    </div>
    <v-carousel v-else cycle height="100%" hide-delimiters show-arrows="hover">
      <v-carousel-item
        v-for="image in carousel"
        height="350"
        :src="image.attributes.url"
        cover
      ></v-carousel-item>
    </v-carousel>

    <v-card-actions>
      <v-btn class="mx-auto" icon @click="show = !show">
        SHOW REMEDY
        <v-icon>{{ show ? "mdi-chevron-up" : "mdi-chevron-down" }}</v-icon>
      </v-btn>
    </v-card-actions>

    <v-expand-transition>
      <div v-show="show">
        <v-divider></v-divider>

        <v-card-text>
          <div class="content">
            <div v-html="content"></div>
          </div>
        </v-card-text>
      </div>
    </v-expand-transition>
  </v-card>
</template>

<style scoped>
.content {
  padding: 50px;
  margin-top: 0px;
}
.content :deep(h1) {
  line-height: 1em;
}
.content :deep(h2) {
  line-height: 2em;
}
.content :deep(h3) {
  margin-top: 1em;
  margin-bottom: 1em;
}
.content :deep(ul) {
  margin-left: 3em;
}

@media screen and (max-width: 600px) {
  .content {
    padding: 0px;
  }
}
</style>
