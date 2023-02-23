<script setup lang="ts">
import { ref } from 'vue'
import product1 from "../../images/image-product-1.jpg"
import product2 from "../../images/image-product-2.jpg"
import product3 from "../../images/image-product-3.jpg"
import product4 from "../../images/image-product-4.jpg"
import product1Thumbnail from "../../images/image-product-1-thumbnail.jpg"
import product2Thumbnail from "../../images/image-product-2-thumbnail.jpg"
import product3Thumbnail from "../../images/image-product-3-thumbnail.jpg"
import product4Thumbnail from "../../images/image-product-4-thumbnail.jpg"
import prev from "../../images/icon-previous.svg"
import next from "../../images/icon-next.svg"

let productImage = $ref(product1)
let dialogImage = $ref(product1)
const scroll = $ref(null)
let dialog = $ref(null)
function nextImage() {
    if(scroll.offsetWidth + scroll.scrollLeft >= scroll.scrollWidth) {
        scroll.scrollLeft = 0
    } else {
        scroll.scrollLeft += scroll.offsetWidth
    }
}
function prevImage() {
    if(scroll.scrollLeft <= 0) {
        scroll.scrollLeft = scroll.scrollWidth
    } else {
        scroll.scrollLeft -= scroll.offsetWidth
    }
}
function prevImageDialog() {
    
    if (dialogImage === product1) {
        dialogImage = product4
    } else if (dialogImage === product2) {
        dialogImage = product1
    } else if (dialogImage === product3) {
        dialogImage = product2
    } else if (dialogImage === product4) {
        dialogImage = product3
    }
}
function nextImageDialog() {
    if (dialogImage === product1) {
        dialogImage = product2
    } else if (dialogImage === product2) {
        dialogImage = product3
    } else if (dialogImage === product3) {
        dialogImage = product4
    } else if (dialogImage === product4) {
        dialogImage = product1
    }
}
function open(product: string) {
    dialogImage = product
    if(!dialog.open) {
        dialog.showModal()
    }
}
</script>

<template>
<dialog class="lightbox overflow-y-hidden" ref="dialog">
    <div class="relative lg:p-16  w-fit">
        <div class="product-container overflow-x-auto overflow-y-hidden relative h-[520px] w-full lg:w-[500px] lg:h-1/2 lg:rounded-md lg:snap-none lg:overflow-x-hidden " ref="scroll" >
            <div class="child h-full  w-full">
            <img @click="open(product1)"   :src="dialogImage" alt="product1" class="product-image rounded-md cursor-pointer" />
            </div>      
        </div>
        <div class="hidden lg:flex w-9/12  pt-4 pr-1 gap-6 absolute h-1/2">
            <div class="img-container w-full">
            <img @click="open(product1)" :class="product1 == dialogImage ? 'active' : ''"  :src="product1Thumbnail" alt="product1" class="thumbnail w-full" />
            </div>
            <div class="img-container w-full">
            <img @click="open(product2)" :class="product2 == dialogImage ? 'active' : ''"  :src="product2Thumbnail" alt="product2" class="thumbnail w-full" />
            </div>
            <div class="img-container w-full">
            <img @click="open(product3)" :class="product3 == dialogImage ? 'active' : ''"  :src="product3Thumbnail" alt="product3" class="thumbnail w-full" />
            </div>
            <div class="img-container w-full">
            <img @click="open(product4)" :src="product4Thumbnail" :class="product4 == dialogImage ? 'active' : ''"  alt="product4" class="thumbnail w-full" />
            </div>
        </div>
        <button @click="prevImageDialog" class=" prev absolute flex justify-center items-center top-1/2 left-0 transform -translate-y-1/2 bg-white rounded-full h-10 w-10 ">
            <img :src="prev" class="" />
            </button>
            <button @click="nextImageDialog" class="next absolute flex justify-center items-center top-1/2 right-0 transform -translate-y-1/2 bg-white rounded-full h-10 w-10">
            <img :src="next"  />
            </button>
            </div>
</dialog>

<div class="relative lg:p-16 m-0  ">
    <div class="product-container overflow-x-auto relative h-[320px] w-full  lg:h-1/3 lg:rounded-md lg:snap-none lg:overflow-x-hidden " ref="scroll" >
        <div class="child  w-full">
        <img @click="open(product1)"  :src="product1" alt="product1" class="product-image cursor-pointer" />
        </div>
        <div class="child  w-full ">
        <img @click="open(product2)" :src="product2" alt="product2" class="product-image" />
        </div><div class="child  w-full ">
        <img @click="open(product3)" :src="product3" alt="product3" class="product-image" />
        </div>
        <div class="child  w-full">
        <img @click="open(product4)" :src="product4" alt="product4" class="product-image" />
        </div>       
    </div>
    <div class="hidden lg:flex w-10/12 lg:pr-8  pt-4 pr-5 gap-6 absolute h-1/2">
        <div class="img-container w-full">
        <img @click="open(product1)" :class="product1 == dialogImage ? 'active' : ''" :src="product1Thumbnail" alt="product1" class="thumbnail w-full cursor-pointer" />
        </div>
        <div class="img-container w-full">
        <img @click="open(product2)" :class="product2 == dialogImage ? 'active' : ''"  :src="product2Thumbnail" alt="product2" class="thumbnail w-full cursor-pointer" />
        </div>
        <div class="img-container w-full">
        <img @click="open(product3)" :class="product3 == dialogImage ? 'active' : ''"  :src="product3Thumbnail" alt="product3" class="thumbnail w-full cursor-pointer" />
        </div>
        <div class="img-container w-full">
        <img @click="open(product4)" :class="product4 == dialogImage ? 'active' : ''"  :src="product4Thumbnail" alt="product4" class="thumbnail w-full cursor-pointer" />
        </div>
    </div>
     <button @click="prevImage" class=" prev absolute flex justify-center items-center top-1/2 left-0 transform -translate-y-1/2 bg-white rounded-full h-10 w-10 lg:hidden">
        <img :src="prev" class="" />
        </button>
        <button @click="nextImage" class="next absolute flex justify-center items-center top-1/2 right-0 transform -translate-y-1/2 bg-white rounded-full h-10 w-10 lg:hidden">
        <img :src="next"  />
        </button>
        </div>
</template>

<style scoped>
    .product-container {
        transition: all 0.8s ease-in;
  overflow-y: hidden;
  white-space: nowrap;
  /** Thank you Sergey! http://blog.gospodarets.com/css-scroll-snap/ **/
    scroll-snap-points-x: repeat(100%);
    scroll-behavior: smooth;
    scroll-snap-type: mandatory;
    -webkit-scroll-snap-type: mandatory;
    -webkit-scroll-snap-points-x: repeat(100%);
    }
    .child {
   position: relative;
  display: inline-block;
  width: 100%;
  height: 100%;
    }
   .img-container {
    height: fit-content;
   }
.img-container:hover {
    outline: 4px solid hsl(26, 100%, 55%);
    opacity: .5;
    border-radius: 10px;
}
  .active {
    outline: 4px solid hsl(26, 100%, 55%);
    opacity: .5;
    border-radius: 10px;
  }
  .lightbox::backdrop {
    background-color: rgba(0,0,0,0.8);
  }
  .lightbox {
    background: transparent;
    border-radius: 20px;
    height: 100%;
  }
</style>
