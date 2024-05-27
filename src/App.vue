<template>
  <div class="container-fluid h-100 p-0">
    <div class="row g-0 h-100">
      <div class="col-8 h-100">
        <div class="large-image">
          <!-- <swiper style="height: 100%" :modules="modules" :autoplay="{ delay: 5000, stopOnLastSlide: false }">
            <swiper-slide v-for="(image, index) in largeImages" :key="index">
              <img :src="image" class="img-fluid" :alt="`large image box 1 ${index + 1}`" />
            </swiper-slide>
          </swiper> -->
          <!-- <iframe width="100%" height="100%" src="https://www.youtube.com/embed/Z3vXqizCuQY?si=fobirZN5BjuXziPY"
            title="YouTube video player" frameborder="0"
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
            referrerpolicy="strict-origin-when-cross-origin" allowfullscreen="no"></iframe> -->

          <div style="position: relative; width: 100%; height: 100%;">
            <iframe style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"
              src="https://www.youtube.com/embed/PPnFaCZSjcc?si=njow-fAjn6gKMgl4" title="YouTube video player"
              frameborder="0"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
              referrerpolicy="strict-origin-when-cross-origin" allowfullscreen>
            </iframe>
          </div>



        </div>
      </div>
      <div class="col-4 d-flex flex-column" style="height: calc(100vh - 0px);">
        <div class="small-image-top" style="height: 50%;">
          <swiper :modules="modules" :autoplay="{ delay: 15000, stopOnLastSlide: false }">
            <swiper-slide v-for="(image, index) in smallBoxImage1" :key="index">
              <img :src="image" class="img-fluid" :alt="`small image box 1 - ${index + 1}`" />
            </swiper-slide>
          </swiper>
        </div>
        <div class="small-image-bottom" style="height: 50%;">
          <swiper :modules="modules" :autoplay="{ delay: 15000, stopOnLastSlide: false }">
            <swiper-slide v-for="(image, index) in smallBoxImage2" :key="index">
              <img :src="image" class="img-fluid" :alt="`small image box 2 - ${index + 1}`" />
            </swiper-slide>
          </swiper>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import { Swiper, SwiperSlide } from "swiper/vue";
import { A11y, Autoplay } from "swiper/modules";
import axios from "axios";

import "swiper/css";

export default {
  components: {
    Swiper,
    SwiperSlide,
  },
  setup() {
    const largeImages = ref([]);

    const smallBoxImage1 = ref([]);
    const smallBoxImage2 = ref([]);

    onMounted(async () => {
      //check localstorage for key store_location
      if (!localStorage.getItem("store_location")) {
        //show alert input location (number)
        const location = prompt("Masukan kode lokasi");

        if (!location) {
          alert("Please enter a valid location");
          //remove localstorage
          localStorage.removeItem("store_location");
          //refresh page
          location.reload();
        }

        //fetch data from api
        const response = await axios.get('http://localhost:3000/api/location/' + location);

        if (response.data.status === true) {
          //save location to localstorage
          localStorage.setItem("store_location", JSON.stringify(response.data.data));
        } else {
          alert("Please enter a valid location");
          //remove localstorage
          localStorage.removeItem("store_location");
          //refresh page
          location.reload();
        }
      }

      //get location from localstorage
      const location = JSON.parse(localStorage.getItem("store_location"));

      //set largeImages
      largeImages.value = location.adsPrimary.map((ad) => ad.file);

      //set smallBoxImage1
      smallBoxImage1.value = location.adsSecondary.filter((ad) => ad.boxNumber === 1).map((ad) => ad.file);

      //set smallBoxImage2
      smallBoxImage2.value = location.adsSecondary.filter((ad) => ad.boxNumber === 2).map((ad) => ad.file);
    });

    //when user hit combination key clear the localstorage
    window.addEventListener("keydown", (e) => {
      if (e.ctrlKey && e.shiftKey && e.key === "C") {
        e.preventDefault();
        localStorage.removeItem("store_location");
        location.reload();
      }
    });

    return {
      largeImages,
      smallBoxImage1,
      smallBoxImage2,
      modules: [A11y, Autoplay],
    };
  },
};
</script>

