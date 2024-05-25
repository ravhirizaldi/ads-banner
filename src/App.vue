<template>
  <div class="container-fluid h-100 p-0">
    <div class="row g-0 h-100">
      <div class="col-8 h-100">
        <div class="large-image">
          <swiper
          style="height: 100%"
            :modules="modules"
            :autoplay="{delay: 3000, stopOnLastSlide: false}"
          >
          <swiper-slide v-for="(image, index) in largeImages" :key="index">
              <img :src="image" class="img-fluid" alt="Slide 1">
            </swiper-slide>
          </swiper>
        </div>
      </div>
      <div class="col-4 d-flex flex-column">
        <div class="small-image-top" v-for="(image, index) in smallImages" :key="index">
          <img :src="image" class="img-fluid" alt="Small Responsive image">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, ref } from 'vue'
import { Swiper, SwiperSlide } from 'swiper/vue';
import { A11y, Autoplay } from 'swiper/modules';

import 'swiper/css';

export default{
  components: {
    Swiper,
    SwiperSlide
  },
  setup() {
    const largeImages = ref([
      'https://fakeimg.pl/1600x1200',
      'https://fakeimg.pl/1600x1200',
      'https://fakeimg.pl/1600x1200',
    ])

    const smallImages = reactive(
      [
        'https://fakeimg.pl/800x600',
        'https://fakeimg.pl/800x600',
      ]
    )
    return {
      largeImages,
      smallImages,
      modules: [A11y, Autoplay],
    }
  }
}
</script>

