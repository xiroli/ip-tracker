<script setup lang="ts">
import { ref } from 'vue'
import minus from "../../images/icon-minus.svg"
import plus from "../../images/icon-plus.svg"
import cart from "../../images/icon-cart.svg"
let quantityCart = $ref(0)
const emit = defineEmits<{
    (event: 'add-to-cart', quantity: number): void,
    (event: 'remove-from-cart', quantity: number): void,
    (event: 'open-cart'): void
}>()
function addItem() {
    quantityCart++
    
}
function removeItem() {
    if(quantityCart > 0) {
        quantityCart--
    }
}
function addCart() {
    emit('add-to-cart', quantityCart)
}
function openCart() {
    emit('open-cart')
}
</script>

<template>
<section class="product p-6">
    
    <p class="logo">SNEAKER COMPANY</p>
    
    <div class="logo-container lg:mt-2 pr-4 ">
    <h1 class="lg:!text-[50px]">Fall Limited Edition Sneakers</h1>
    </div>
    <p class="text mt-4 lg:mt-6 lg:!text-[20px] lg:mb-5">These low-profile sneakers are your perfect casual wear companion. 
    Featuring a durable rubber outer sole, they’ll withstand everything the weather
    can offer.</p>
    <div class="details flex justify-between flex-row lg:flex-col">
        <div class="flex gap-4 flex-row">
        <p class="sum lg:text-start">$125.00</p>
        <span class="discount rounded-sm flex items-center justify-center px-1">50%</span>
        </div>
        <p class="old lg:text-start">$250.00</p>
    </div>
    <div class="quantity rounded-md mb-2 lg:mt-4 flex justify-between p-4">
        
    <img @click="removeItem" :src="minus" alt="minus" class="h-full mt-2 lg:mt-5 cursor-pointer" />
    <p class="lg:mt-2">{{quantityCart}}</p>
    <img @click="addItem" :src="plus" alt="plus" class="h-full mt-2 lg:mt-5 cursor-pointer" />
   
    <button @click="addCart" class="cursor-pointer add-to-cart hidden lg:flex flex-row gap-4 p-4 rounded-md w-1/2 justify-center text-white">
        <svg width="22" height="20" xmlns="http://www.w3.org/2000/svg"><path d="M20.925 3.641H3.863L3.61.816A.896.896 0 0 0 2.717 0H.897a.896.896 0 1 0 0 1.792h1l1.031 11.483c.073.828.52 1.726 1.291 2.336C2.83 17.385 4.099 20 6.359 20c1.875 0 3.197-1.87 2.554-3.642h4.905c-.642 1.77.677 3.642 2.555 3.642a2.72 2.72 0 0 0 2.717-2.717 2.72 2.72 0 0 0-2.717-2.717H6.365c-.681 0-1.274-.41-1.53-1.009l14.321-.842a.896.896 0 0 0 .817-.677l1.821-7.283a.897.897 0 0 0-.87-1.114ZM6.358 18.208a.926.926 0 0 1 0-1.85.926.926 0 0 1 0 1.85Zm10.015 0a.926.926 0 0 1 0-1.85.926.926 0 0 1 0 1.85Zm2.021-7.243-13.8.81-.57-6.341h15.753l-1.383 5.53Z" fill="white" fill-rule="nonzero"/></svg>
        Add to Cart</button>
  
    </div>
      <button @click="addCart" class="cursor-pointer add-to-cart flex lg:hidden flex-row gap-4 p-4 rounded-md w-full justify-center text-white">
        <svg width="22" height="20" xmlns="http://www.w3.org/2000/svg"><path d="M20.925 3.641H3.863L3.61.816A.896.896 0 0 0 2.717 0H.897a.896.896 0 1 0 0 1.792h1l1.031 11.483c.073.828.52 1.726 1.291 2.336C2.83 17.385 4.099 20 6.359 20c1.875 0 3.197-1.87 2.554-3.642h4.905c-.642 1.77.677 3.642 2.555 3.642a2.72 2.72 0 0 0 2.717-2.717 2.72 2.72 0 0 0-2.717-2.717H6.365c-.681 0-1.274-.41-1.53-1.009l14.321-.842a.896.896 0 0 0 .817-.677l1.821-7.283a.897.897 0 0 0-.87-1.114ZM6.358 18.208a.926.926 0 0 1 0-1.85.926.926 0 0 1 0 1.85Zm10.015 0a.926.926 0 0 1 0-1.85.926.926 0 0 1 0 1.85Zm2.021-7.243-13.8.81-.57-6.341h15.753l-1.383 5.53Z" fill="white" fill-rule="nonzero"/></svg>
        Add to Cart</button>
    
</section>
</template>

<style scoped>
.quantity {
    background: hsl(223, 64%, 98%);
}
.add-to-cart {
    background: hsl(26, 100%, 55%);
}

.add-to-cart:hover {
    opacity: 0.8;
}
span {
    background-color: hsl(25, 100%, 94%);
}
.logo {
    color: hsl(26, 100%, 55%);
    text-align: start;
}
.text {
    font-size: 16px;
    text-align: start;
    color: hsl(219, 9%, 45%);
}
h1 {
    text-align: start;
    color: hsl(220, 13%, 13%);
    font-weight: 700;
    font-size: 25px;
    font-family: Kumbh Sans;
    line-height: 1.1;
}
.sum {
    font-size: 25px;
    font-weight: 700;
    font-family: Kumbh Sans;
    color: hsl(220, 13%, 13%);
}
.discount {
    font-size: 17px;
    text-align: center;
    font-weight: 700;
    font-family: Kumbh Sans;
    color: hsl(26, 100%, 55%);
}
.old {
    position: relative;
    color:hsl(220, 14%, 75%);
}
.old::after{
    @apply lg:!w-16 lg:!top-3;
    content: "";
    position: absolute;
    top: 30%;
    left: 0;
    width: 100%;
    height: 1px;
    background-color: hsl(220, 13%, 13%);
}
</style>
