<template>
    <div class="flex py-6 px-3 bg-weather-secondary rounded-md shadow-md cursor-pointer">
        <div class="flex flex-col flex-1">
            <h2 class="text-3xl">{{ city.city }}</h2>
            <h3>{{ city.state }}</h3>
        </div>
        <div class="flex flex-col gap-2">
            <p class="text-3xl self-end">
                {{ Math.round((city.weather.current.temp - 32) / (9 / 5)) }}&deg
            </p>
            <div class="flex gap-2">
                <span class="text-xs">
                    Feels like {{ Math.round((city.weather.current.feels_like - 32) / (9 / 5)) }}&deg
                </span>
            </div>
        </div>
    </div>
</template>

<script setup>
defineProps({
    city: {
        type: Object,
        default: () => { },
    }
})
// console.log(city)
</script>

