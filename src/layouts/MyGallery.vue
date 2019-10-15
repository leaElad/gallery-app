<template>
  <div>
    <div class="header q-ml-xl">
      <h3>Welcome to my gallery app</h3>
    </div>
    <div class="q-pa-md">
      <q-infinite-scroll @load="onLoad" :offset="100">
        <div class="row">
          <div v-for="(item, index) in items" :key="index" class="col-3 q-ma-md">
            <q-img :src="getImageUrl(item)" :ratio="1" @click="focusOnImg(item,index)" />
          </div>
        </div>
        <template v-slot:loading>
          <div class="row justify-center q-my-md">
            <q-spinner-dots color="primary" size="40px" />
          </div>
        </template>
      </q-infinite-scroll>
    </div>
    <q-dialog v-model="carousel">
      <photo-modal :left-photo="leftPhoto" :right-photo="rightPhoto" :main-photo="mainPhoto"></photo-modal>
    </q-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [],
      carousel: false,
      mainPhoto: "",
      leftPhoto: "",
      rightPhoto: "",
      galleryIndex: 0
    };
  },

  methods: {
    onLoad(index, done) {
      const galleries = [
        "72157647277042064",
        "72157649026354681",
        "72157646433779973",
        "72157647542685978"
      ];
      this.$axios
        .get(
          `https://api.flickr.com/services/rest/?method=flickr.galleries.getPhotos&api_key=76f2073c1889215b74ff0a5709272231&gallery_id=66911286-${
          galleries[this.galleryIndex % galleries.length]
          }&format=json&nojsoncallback=1`
        )
        .then(response => {
          const photos = response.data.photos.photo;
          if (!this.items.length) {
            this.items = photos;
          } else {
            this.items.push(...photos);
          }
          this.galleryIndex++;
          done();
        });
    },
    focusOnImg(item, index) {
      const rightIndex = index + 1 === this.items.length ? 0 : index + 1;
      const leftIndex = index - 1 < 0 ? this.items.length - 1 : index - 1;
      this.mainPhoto = this.getImageUrl(item);
      this.leftPhoto = this.getImageUrl(this.items[leftIndex]);
      this.rightPhoto = this.getImageUrl(this.items[rightIndex]);
      this.carousel = true;
    },
    getImageUrl(photoDetails) {
      return `https://farm${photoDetails.farm}.staticflickr.com/${photoDetails.server}/${photoDetails.id}_${photoDetails.secret}.jpg`;
    }
  }
};
</script>
