<script setup lang="ts">
<<<<<<< HEAD
 
import { onMounted, ref, watch } from "vue"
import "leaflet/dist/leaflet.css";
import leaflet from "leaflet";
import maxboxgl from "mapbox-gl"
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
    center: [48.773748,  9.182143],
    zoom: 13
}).locate({setView: true, maxZoom: 16});
navigator.geolocation.getCurrentPosition((position) => {
    console.log(position.coords.latitude, "lat")
    console.log(position.coords.longitude, "long")
    leaflet.marker([position.coords.latitude, position.coords.longitude], {icon: icon}).addTo(map)
})

   const baseUrl = "https://api.ipgeolocation.io/ipgeo?apiKey=c8940a7c6394428eaccf17f6b32e7bd4"
 
   maxboxgl.accessToken = 'pk.eyJ1IjoieGlyb2xpIiwiYSI6ImNsZWZ3OWNuaDAzZnIzcW1jNWk4eW00a2cifQ.wUcS45yu06EjqvHt2m2lRA';
// const maps = new maxboxgl.Map({
//   container: 'mapContainer',
//   style: 'mapbox://styles/mapbox/streets-v11'
// });
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
let timezone = $ref(0)
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
function test() {
    mapContainer.panTo(new L.LatLng(42.737, -73.923), 8);
console.log(mapContainer.getCenter(), "center")

}
function search() {
    const baseUrl = "https://api.ipgeolocation.io/ipgeo?apiKey=c8940a7c6394428eaccf17f6b32e7bd4&ip="
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
            timezone = res.data.time_zone.offset
            isp = res.data.isp
            locations = [res.data.latitude, res.data.longitude]
            console.log(locations)
    map.panTo(leaflet.latLng(locations[0], locations[1]), 8);
    leaflet.marker([locations[0], locations[1]], {icon: icon}).addTo(map)
        })

    } else if (domain.test(input)) {
        console.log("domain")
        axios.get(baseUrl + input).then((res: Data) => {
            ip = res.data.ip
            location = res.data.city
            timezone = res.data.time_zone.offset
            isp = res.data.isp
            locations = [res.data.latitude, res.data.longitude]
            map.panTo(leaflet.latLng(locations[0], locations[1]), 8);
    leaflet.marker([locations[0], locations[1]], {icon: icon}).addTo(map)
        })
    } else if (ipv6.test(input)) {
        console.log("ipv6")
        axios.get(baseUrl + input).then((res: Data) => {
            ip = res.data.ip
            location = res.data.city
            timezone = res.data.time_zone.offset
            isp = res.data.isp
            locations = [res.data.latitude, res.data.longitude]
            map.panTo(leaflet.latLng(locations[0], locations[1]), 8);
    leaflet.marker([locations[0], locations[1]], {icon: icon}).addTo(map)

        })
    } else {
        console.log("invalid")
        alert("invalid input")
    }
   
}
</script>

<template>
    <button @click="test">test</button>
    <main class="main-content flex flex-col relative h-full">
        <div class="input-container relative flex h-2/5 ">
            <div class="text-area">
                <h1 class="text-center text-sm mt-4 lg:text-2xl absolute w-full z-10">IP Adress tracker</h1>
            </div>
            <div class="text-input p-4 flex justify-center items-center w-full">

                <input type="text" v-model="input" class="w-full self-center justify-self-center p-4 rounded-md lg:w-2/3 h-10 z-10">
                <button @click="search" class="w-10 h-10 bg-black z-10 flex items-center justify-center">
                <img :src="arrow" class="w-4 h-4 z-10">
                </button>
            </div>
<img :src="patternLg" class="absolute w-full object-cover h-full hidden lg:block">
<img :src="patternSm" class="absolute object-cover w-full h-full block lg:hidden">
        </div>
        <div class="result-box p-4 absolute top-1/3 left-1/2 -translate-x-1/2  z-30 bg-white rounded-md">
            <div class="results flex gap-4 flex-col lg:flex-row">


                <div class="ip break">
                    <h2 class="text-center text-sm lg:text-2xl">IP Adress</h2>
                    <p class="text-center text-sm lg:text-2xl">{{ip}}</p>
                </div>
                <div class="divider hidden lg:block h-20 w-1 self-center bg-black" />
                <div class="location break">
                    <h2 class="text-center text-sm lg:text-2xl">Location</h2>
                    <p class="text-center text-sm lg:text-2xl">{{location}}</p>
                </div>
                <div class="divider hidden lg:block h-20 w-1 self-center bg-black" />
    
                <div class="timezone break">
                    <h2 class="text-center text-sm lg:text-2xl">Timezone</h2>
                    <p class="text-center text-sm lg:text-2xl">GMT {{timezone}}</p>
                </div>
                <div class="divider hidden lg:block h-20 w-1 self-center bg-black" />
    
                <div class="isp">
                    <h2 class="text-center text-sm lg:text-2xl">ISP</h2>
                    <p class="text-center text-sm lg:text-2xl">{{isp}}</p>
                </div>
            </div>

        </div>
        <div id="mapContainers" class="h-3/5">
            <div id="mapContainer" ref="mapContainer" class="h-full    "></div>

           

        </div>
=======
 import menuIcon from "../images/icon-menu.svg"
 import cart from "../images/icon-cart.svg"
 import avatar from "../images/image-avatar.png" 
 import iconClose from "../images/icon-close.svg"
 import ImageCarousel from "./components/ImageCarousel.vue"
    import Product from "./components/Product.vue"
    import product1 from "../images/image-product-1.jpg"
    import deleteIcon from "../images/icon-delete.svg"
import { onMounted, ref, watch } from "vue"
    let menu = $ref(false)
    let dialog = $ref(null)
    let cartUser = $ref(false)
    let cartAmount = $ref(0)
    function dialogs() {
        menu = !menu
        if(!dialog.open) {
            dialog.showModal()
        } 
    }
    onMounted(() => {
        
        console.log(dialog)
        
        // dialog.open()
    })
    function close() {
        dialog.close()
    }
    function addCart(q: number) {
        cartAmount += q
    }
    function removeCart() {
        cartAmount--
    }
    function removeItem() {
        cartAmount != 0 ? cartAmount-- : cartAmount = 0
    }
</script>

<template>
    <dialog class="sidebar relative left-0 w-3/4 h-full" ref="dialog">
        <div class="sidebar-content flex flex-col absolute top-20 gap-4 left-6 h-full bg-white">
            <a>Collections</a>
            <a>Men</a>
            <a>Woman</a>
            <a>About</a>
            <a>Contact</a>
            </div>
            <button @click="close" class="absolute left-2 top-2 p-4">
                <img :src="iconClose" alt="menu icon" class="lg:hidden max-h-max" />
            </button>
    </dialog>
    <header class="header flex flex-row gap-2 p-8">
        <div class="mt-2">
            <button @click="dialogs">
        <img :src="menuIcon" alt="menu icon" class="lg:hidden max-h-max" />
            </button>
       </div>
        <nav class="nav flex w-full ">
            <p class="logo mr-6">sneakers</p>
            <div class="gap-5 flex-row mt-1 hidden lg:flex">
            <a class="desktop-nav">Collections</a>
            <a class="desktop-nav">Men</a>
            <a class="desktop-nav">Woman</a>
            <a class="desktop-nav">About</a>
            <a class="desktop-nav">Contact</a>
            </div>
            <div class="flex justify-end w-full gap-4">
            <div class="relative">
            <img @click="cartUser = !cartUser" :src="cart" alt="cart icon" class="cart-icon-container h-5 w-5 lg:mt-2 lg:mr-2 relative" />
            <div class="absolute left-3 -top-1 circle flex justify-center items-center h-5 w-5" v-if="cartAmount > 0">
                <p class="text-center text-white">{{cartAmount}}</p>
</div>
                </div>
           
            
            <img :src="avatar" alt="avatar" class="h-5 w-5 lg:h-10 lg:w-10 avatar " />
            
            </div>
        </nav>
        
    </header>
    <div class="container pt-8 pl-2 pr-6 ">
    <div class="cart w-full lg:w-1/3 lg:right-0 h-56  mt-2 absolute z-10 bg-white overflow-hidden " v-if="cartUser">
        <h1 class="text-start cart-text">Cart</h1>
        <div class="h-[0.5px] mt-5 divider bg-gray-200 w-full" />
            <p class="empty mt-10" v-if="cartAmount == 0">Your cart is empty.</p>

        <div class="item-container h-full flex flex-col px-4 " v-if="cartAmount > 0">
              <div class="flex flex-row gap-2">
                <img class="h-10 h-10 mt-2" :src="product1" />
              
                <div class="flex flex-col w-full gap-1">

                    <p class="text-start text-container">Fall Limited Edition Sneakers</p>
                    <div class="flex flex-row w-full">
                    <p class="text-start  text-container mr-2">$125.00 x {{cartAmount}}</p>
                    <h2>${{cartAmount * 125}}.00</h2>
                    </div>
                    

                </div>
                 <div class="trash">
                    <img @click="removeItem" :src="deleteIcon" alt="delete" class="h-5 w-5 mt-4" />
                    </div>
                  </div>
           
                    <div class="basis-1">

            <button class="checkout p-4  text-white" v-if="cartAmount > 0">Checkout</button>
                    </div>

            </div>
   

         </div>

    </div>
    <main class="flex flex-col lg:flex-row">
   <section class="image-carousel lg:w-1/2 lg:p-8 ">
    <ImageCarousel />
   </section>
   <section class="product lg:pt-8 lg:w-1/2 lg:mt-16">
    <Product @add-to-cart="addCart" @remove-from-cart="removeCart" @open-cart="cartUser = !cartUser" />
    </section>
>>>>>>> 6192fe1325b300777c311a67554347e281b6f0e7
    </main>
</template>

<style scoped>
<<<<<<< HEAD
h1 {
    font-weight: 700;
    color: hsl(0, 0%, 100%);
=======
.cart-icon-container {
    cursor: pointer;
}
.cart-icon-container:hover {
}
.desktop-nav {
    font-size: 16px;
    font-family: Kumbh Sans;
    color: hsl(219, 9%, 45%);
    position: relative;
}
.avatar {
    cursor: pointer;
    position: relative;
}
.avatar:hover {
    border-radius: 100%;
    border: 3px solid hsl(26, 100%, 55%);
}
.desktop-nav:hover::after {
    content: '';
    position: absolute;
    bottom: -10px;
    left: 0;
    width: 100%;
    height: 2px;
    border: 1px solid hsl(26, 100%, 55%);
}
h2 {
    font-family: Kumbh Sans;
    font-weight: 700;
    font-size: 16px;
    color: hsl(220, 13%, 13%);
}
.checkout {
    background: hsl(26, 100%, 55%);
    border-radius: 10px;
    width: 100%;
    margin-top: 20px;
}
.circle {
    background: hsl(26, 100%, 55%);
    border-radius: 50px;
}
.empty, .text-container  {
    color:  hsl(219, 9%, 45%);
}
.cart-text {
    font-size: 20px;
    font-family: Kumbh Sans;
    font-weight: 700;
    color: hsl(220, 13%, 13%);
}
a {
    font-size: 16px;
    font-family: Kumbh Sans;
    font-weight: 700;
    color: hsl(220, 13%, 13%);
    cursor: pointer;
    position: relative;
    width: 100%;
    text-align: start;
}
a:hover::after {
    content: "";
    margin-top: 5px;
    border: 1px solid black;
    position: absolute;
    bottom: 0;
    left: 0;
    width: 80%;
}
    .logo {
    font-weight: 700;
    color: black;
    font-size: 20px;
    font-family: Kumbh Sans;
}
dialog {
    transition: all 0.3s ease-in-out;
    max-height: 100vh;
}
dialog:modal {
    position: absolute;
    margin: 0px;
    max-width: auto;
    max-height: auto;
    user-select: text;
    visibility: visible;
    overflow: auto;
}
.cart {
    max-width: 95%;
>>>>>>> 6192fe1325b300777c311a67554347e281b6f0e7
}
</style>


<<<<<<< HEAD

- Mobile: 375px
- Desktop: 1440px

## Colors

- Very Dark Gray: hsl(0, 0%, 17%)
- Dark Gray: hsl(0, 0%, 59%)
=======
### Primary

- Orange: hsl(26, 100%, 55%)
- Pale orange: hsl(25, 100%, 94%)

### Neutral

- Very dark blue: hsl(220, 13%, 13%)
- Dark grayish blue: hsl(219, 9%, 45%)
- Grayish blue: hsl(220, 14%, 75%)
- Light grayish blue: hsl(223, 64%, 98%)
- White: hsl(0, 0%, 100%)
- Black (with 75% opacity for lightbox background): hsl(0, 0%, 0%)
>>>>>>> 6192fe1325b300777c311a67554347e281b6f0e7

## Typography

### Body Copy

<<<<<<< HEAD
- Font size (text input): 18px

### Font

- Family: [Rubik](https://fonts.google.com/specimen/Rubik)
- Weights: 400, 500, 700
=======
- Font size (paragraph): 16px

### Font

- Family: [Kumbh Sans](https://fonts.google.com/specimen/Kumbh+Sans)
- Weights: 400, 700
>>>>>>> 6192fe1325b300777c311a67554347e281b6f0e7
