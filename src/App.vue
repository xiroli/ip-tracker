<script setup lang="ts">

import { onMounted, ref, watch } from "vue"
import "leaflet/dist/leaflet.css";
import leaflet from "leaflet";
// import maxboxgl from "mapbox-gl"
import patternLg from "../images/pattern-bg-desktop.png"
import patternSm from "../images/pattern-bg-mobile.png"
import arrow from "../images/icon-arrow.svg"
import axios from "axios"
let latlng = leaflet.latLng(51.23, 6.73);

let mapContainer = $ref(null);
let map: any
const icon = leaflet.icon({
    iconUrl: '../images/icon-location.svg',
    iconSize: [46, 56],
    iconAnchor: [22, 94],
    popupAnchor: [-3, -76],
    shadowSize: [68, 95],
    shadowAnchor: [22, 94]
});
onMounted(() => {
    map = leaflet.map('mapContainer', {
        center: [48.773748, 9.182143],
        zoom: 13
    }).locate({ setView: true, maxZoom: 16 });
    navigator.geolocation.getCurrentPosition((position) => {
        console.log(position.coords.latitude, "lat")
        console.log(position.coords.longitude, "long")
        leaflet.marker([position.coords.latitude, position.coords.longitude], { icon: icon }).addTo(map)
    })

    const baseUrl = "https://api.ipgeolocation.io/ipgeo?apiKey=c8940a7c6394428eaccf17f6b32e7bd4"

    // maxboxgl.accessToken = 'pk.eyJ1IjoieGlyb2xpIiwiYSI6ImNsZWZ3OWNuaDAzZnIzcW1jNWk4eW00a2cifQ.wUcS45yu06EjqvHt2m2lRA';
    // // const maps = new maxboxgl.Map({
    // //   container: 'mapContainer',
    // //   style: 'mapbox://styles/mapbox/streets-v11'
    // // });
    leaflet.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
        maxZoom: 19,
        attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
    }).addTo(map);

    //move map to different location

    axios.get(baseUrl + input).then((res: Data) => {
        console.log(res.data, "Mount")
    })
})

let input = $ref("")
let ip = $ref("")
let location = $ref("")
let timezone = $ref("")
let isp = $ref("")
type Data = {
    data: {
        ip: string,
        city: string,
        time_zone: {
            offset: number
        },
        isp: string,
        latitude: number,
        longitude: number
    }
}
function search() {
    const apiKey = import.meta.env.VITE_API_KEY
    const baseUrl = "https://api.ipgeolocation.io/ipgeo?apiKey=" + apiKey + "&ip="
    let locations = []
    //regexp for ipv4
    const ipv4 = /^((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/
    //regexp for domain
    const domain = /^([a-z0-9]+(-[a-z0-9]+)*\.)+[a-z]{2,}$/
    //regexp for ipv6
    const ipv6 = /^([0-9a-f]{1,4}:){7,7}[0-9a-f]{1,4}$|^([0-9a-f]{1,4}:){1,7}:$|^([0-9a-f]{1,4}:){1,6}:[0-9a-f]{1,4}$|^([0-9a-f]{1,4}:){1,5}(:[0-9a-f]{1,4}){1,2}$|^([0-9a-f]{1,4}:){1,4}(:[0-9a-f]{1,4}){1,3}$|^([0-9a-f]{1,4}:){1,3}(:[0-9a-f]{1,4}){1,4}$|^([0-9a-f]{1,4}:){1,2}(:[0-9a-f]{1,4}){1,5}$|^[0-9a-f]{1,4}:((:[0-9a-f]{1,4}){1,6})$|^:((:[0-9a-f]{1,4}){1,7}|:)$/
    //check if input is ipv4 or domain or ipv6
    if (ipv4.test(input)) {
        console.log("ipv4")
        axios.get(baseUrl + input).then((res: Data) => {
            console.log(res.data)
            ip = res.data.ip
            location = res.data.city
            timezone = "GMT " + res.data.time_zone.offset
            isp = res.data.isp
            locations = [res.data.latitude, res.data.longitude]
            console.log(locations)
            map.panTo(leaflet.latLng(locations[0], locations[1]), 8);
            leaflet.marker([locations[0], locations[1]], { icon: icon }).addTo(map)
        })

    } else if (domain.test(input)) {
        console.log("domain")
        axios.get(baseUrl + input).then((res: Data) => {
            ip = res.data.ip
            location = res.data.city
            timezone = "GMT " +  res.data.time_zone.offset
            isp = res.data.isp
            locations = [res.data.latitude, res.data.longitude]
            map.panTo(leaflet.latLng(locations[0], locations[1]), 8);
            leaflet.marker([locations[0], locations[1]], { icon: icon }).addTo(map)
        })
    } else if (ipv6.test(input)) {
        console.log("ipv6")
        axios.get(baseUrl + input).then((res: Data) => {
            ip = res.data.ip
            location = res.data.city
            timezone = "GMT " +  res.data.time_zone.offset
            isp = res.data.isp
            locations = [res.data.latitude, res.data.longitude]
            map.panTo(leaflet.latLng(locations[0], locations[1]), 8);
            leaflet.marker([locations[0], locations[1]], { icon: icon }).addTo(map)

        })
    } else {
        alert("invalid input")
    }

}
</script>

<template>
    <main class="main-content flex flex-col relative h-full">
        <div class="input-container relative flex h-2/5 ">
            <div class="text-area">
                <h1 class="text-center text-sm mt-4 lg:text-2xl absolute w-full z-10">IP Adress tracker</h1>
            </div>
            <div class="text-input p-4 flex justify-center items-center w-full">

                <input type="text" v-model="input"
                    class="w-full self-center justify-self-center p-4 rounded-md lg:w-1/4 h-10 z-10 bg-white">
                <button @click="search" class="w-10 h-10 bg-black z-10 flex items-center rounded-md justify-center">
                    <img :src="arrow" class="w-4 h-4 z-10">
                </button>
            </div>
            <img :src="patternLg" class="absolute w-full object-cover h-full hidden lg:block">
            <img :src="patternSm" class="absolute object-cover w-full h-full block lg:hidden">
        </div>
        <div class="result-box p-4 absolute top-1/4 lg:top-1/3 left-1/2 w-10/12 items-center -translate-x-1/2 h-[300px] lg:h-[150px] z-40 bg-white rounded-md">
            <div class="results w-full flex justify-around gap-4 h-full flex-col lg:flex-row">


                <div class="ip break">
                    <h2 class="text-center text-sm lg:text-2xl">IP Adress</h2>
                    <p class="text-center text-sm lg:text-2xl font-bold">{{ ip }}</p>
                </div>
                <div class="divider hidden lg:block h-24 w-1 self-center bg-black" />
                <div class="location break">
                    <h2 class="text-center text-sm lg:text-2xl">Location</h2>
                    <p class="text-center text-sm lg:text-2xl font-bold">{{ location }}</p>
                </div>
                <div class="divider hidden lg:block h-24 w-1 self-center bg-black" />

                <div class="timezone break">
                    <h2 class="text-center text-sm lg:text-2xl">Timezone</h2>
                    <p class="text-center text-sm lg:text-2xl font-bold"> {{ timezone }}</p>
                </div>
                <div class="divider hidden lg:block h-24 w-1 self-center bg-black" />

                <div class="isp">
                    <h2 class="text-center text-sm lg:text-2xl">ISP</h2>
                    <p class="text-center text-sm lg:text-2xl font-bold">{{ isp }}</p>
                </div>
            </div>

        </div>
        <div id="mapContainers" class="h-3/5 z-0">
            <div id="mapContainer" ref="mapContainer" class="h-full    "></div>



        </div>
    </main>
</template>

<style scoped>
h1 {
    font-weight: 700;
    color: hsl(0, 0%, 100%);
}

h2 {
    font-weight: 500;
    color: hsl(0, 0%, 59%);
    font-size: 14px;
}
</style>
