<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const prevDate = ref(new Date());
const endDate = ref(new Date());
const pictures = ref([]);

// Update prevDate and endDate on component mount
    onMounted(async () => {
      prevDate.value.setDate(prevDate.value.getDate() - 7);
      endDate.value.setDate(endDate.value.getDate() - 1);
      const result = await axios.get(`https://api.nasa.gov/planetary/apod?api_key=HkCDh9mKfRedC5ZVCxbhmsiqWsr2UtFtUhahnuSO&start_date=${prevDate.value.toISOString().slice(0, 10)}&end_date=${endDate.value.toISOString().slice(0, 10)}`);
      pictures.value = result.data;
      console.log(pictures.value);
      console.log(result);
    });

    const isVideo = (url) => {
      return url.includes('youtube.com') || url.includes('vimeo.com');
    };

</script>

<template>
  <div class="prev-pic-div">
    <div v-for="picture in pictures" :key="picture.date" class="prev-pic">
      <div class="card" style="width: 18rem;">
        <a v-if="!isVideo(picture.url)" :href="picture.hdurl" target="_blank">
          <img :src="picture.hdurl" class="card-img-top space-pic">
        </a>
        <a v-else :href="picture.url" target="_blank">
          <iframe :src="picture.url" class="card-img-top space-pic" frameborder="0" allowfullscreen></iframe>
        </a>
        <div class="card-body">
          <p class="picture-title"><strong>Title:</strong> {{ picture.title }}</p>
          <p class="picture-date"><strong>Date:</strong> {{ picture.date }}</p>
          <div data-bs-theme="dark" class="dropdown-center">
            <button class="btn btn-secondary dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false">
              More info
            </button>
            <ul class="dropdown-menu">
              <li><p class="card-text">{{ picture.explanation }}</p></li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>