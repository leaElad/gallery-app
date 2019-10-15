<template>
  <q-carousel
    swipeable
    animated
    v-model="slide"
    infinite
    thumbnails
    style="width: 100%; height: 90%; box-shadow: none;"
  >

    <q-carousel-slide
      :name="1"
      :img-src="leftPhoto"
      style="background-size: contain; background-repeat: no-repeat;"
    >
    </q-carousel-slide>
    <q-carousel-slide
      :name="2"
      :img-src="mainPhoto"
      style="background-size: contain; background-repeat: no-repeat;"
    >
    </q-carousel-slide>
    <q-carousel-slide
      :name="3"
      :img-src="rightPhoto"
      style="background-size: contain; background-repeat: no-repeat;"
    >
    </q-carousel-slide>
  </q-carousel>
</template>




<script lang="ts">
export default {
  name: "PhotoModal",
  props: {
    mainPhoto: String,
    leftPhoto: String,
    rightPhoto: String
  },
  // props: {
  //   mainPhoto: {
  //     type: String
  //   },
  //   leftPhoto: {
  //     type: String
  //   },
  //   rightPhoto: {
  //     type: String
  //   }
  // },

  data() {
    return {
      slide: 2
    };
  }
};
</script>