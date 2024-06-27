<script>
import axios from 'axios';
import LoadPrevWeek from './components/loadPrevWeek.vue';


export default {
    name: "App",
    data() {
        return {
            picture: null,
            title: null,
            description: null
        };
    },
    async mounted() {
        let result = await axios.get("https://api.nasa.gov/planetary/apod?api_key=HkCDh9mKfRedC5ZVCxbhmsiqWsr2UtFtUhahnuSO");
        this.picture = result.data.hdurl;
        this.title = result.data.title;
        this.description = result.data.explanation;
    },
    components: { LoadPrevWeek }
}
</script>

<template>
    <h1>{{ this.title }}</h1>
    <p>{{ this.description }}</p>
    <img :src="this.picture" class="img-fluid">
    <LoadPrevWeek />
</template>