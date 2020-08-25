<template>
  <div class="container">
    <q-banner inline-actions class="text-white bg-red q-banner" v-if="rejectMsg !== null">
      Не удалось отгрузить список.
      <template v-slot:action>
        <q-btn flat color="white" label="Повторить попытку" @click="location.reload()" />
      </template>
    </q-banner>

    <q-carousel
      v-model="slide"
      transition-prev="scale"
      transition-next="scale"
      swipeable
      animated
      infinite
      control-color="white"
      padding
      arrows
      class="bg-primary text-white shadow-1 rounded-borders carusel-size"
      v-if="rejectMsg === null"
    >
      <q-carousel-slide
        v-bind:name="json.id"
        v-bind:img-src="json.url"
        class="column no-wrap flex-center"
        v-for="(json, index) in allJson"
        v-bind:key="index"
        style="background-size: contain;
    background-repeat: no-repeat;"
      >
        <div class="absolute-bottom custom-caption">
          <div class="text-subtitle1">{{json.title}}</div>
        </div>
      </q-carousel-slide>
    </q-carousel>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      allJson: null,
      rejectMsg: null,
      slide: 501,
    };
  },

  created() {
    axios
      .get("https://jsonplaceholder.typicode.com/photos")
      .then((resolve) =>
        resolve.data.filter((elem) => 500 < elem.id && 511 > elem.id)
      )
      .then((resolve) => (this.allJson = resolve))
      .catch((reject) => (this.rejectMsg = reject));
  },
};
</script>

<style>
.container {
  max-width: 1000px;
  margin: auto;
}

.q-banner {
  margin-top: 40px;
}
.custom-caption {
  text-align: center;
  padding: 12px;
  color: white;
  background-color: rgba(0, 0, 0, 0.3);
}

.carusel-size {
  height: 600px !important;
  max-width: 600px !important;
  margin: auto;
}
</style>
