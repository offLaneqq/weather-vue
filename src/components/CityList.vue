<template>
    <div v-for="city in savedCities" :key="city.id">
        <CityCard :city="city" @click="goToCityView(city)" />
    </div>

    <p v-if="savedCities.length === 0">
        No location added. To start tracking a location, search in the field above.
    </p>
</template>

<script setup>
import { ref } from 'vue';
import CityCard from './CityCard.vue'
import axios from 'axios';
import { useRouter } from 'vue-router';

const savedCities = ref([])
const getCities = async () => {
    if (localStorage.getItem('savedCities')) {
        savedCities.value = JSON.parse(localStorage.getItem('savedCities'))

        const request = []
        savedCities.value.forEach(city => {
            request.push(
                axios.get(`https://api.openweathermap.org/data/3.0/onecall?lat=${city.coords.lat}&lon=${city.coords.lng}&appid=2bc1dad6944a8a270aa9020e951129c8&units=imperial`)
            )
        })

        const weatherData = await Promise.all(request)

        // Delay
        await new Promise((res) => setTimeout(res, 1000))

        weatherData.forEach((value, index) => {
            savedCities.value[index].weather = value.data
        })
    }
}
await getCities()

const router = useRouter()
const goToCityView = (city) => {
    router.push({
        name: 'cityView',
        params: { state: city.state, city: city.city },
        query: { id: city.id, lat: city.coords.lat, lng: city.coords.lng }
    })
}
</script>
