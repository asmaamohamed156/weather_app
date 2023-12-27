<template>
    <div v-for="city in savedCities" :key="city.id">
        <CityCard :city="city" @click="goToCityView(city)"/>
    </div>

    <p v-if="savedCities.length===0">
        No location added, To start tracking alocation, Search in the fieldabove.
    </p>
</template>

<script setup>
import axios from 'axios';
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import CityCard from './CityCard.vue'
const savedCities=ref([]);
const getCities=async ()=>{
    if(localStorage.getItem('savedCities')){
        savedCities.value=JSON.parse(localStorage.getItem('savedCities'));
    };
    const requests=[];
    savedCities.value.forEach((city)=>{
        requests.push(
            axios.get(
                `https://api.openweathermap.org/data/2.5/weather?lat=${city.coords.lat}&lon=${city.coords.lng}&exclude={part}&appid=ecfadf86f3d0edd2d82de5ae6cd4ff86&units=imperial`
                
            )
        );
    });
    const weatherData=await Promise.all(requests);

    //Flicker delay
    await new Promise((res)=>setTimeout(res,500));
    weatherData.forEach((value,index)=>{
        savedCities.value[index].weather=value.data;
    })
}
await getCities();

const router=useRouter();
const goToCityView=(city)=>{
    router.push({
        name:"cityView",
        params: { country: city.country, state: city.state, city: city.city },
        query: {
        id: city.id,
        lat: city.coords.lat,
        lng: city.coords.lng,
        },
    })
}
</script>

