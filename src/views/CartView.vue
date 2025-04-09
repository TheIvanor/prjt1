<template>
  <div class="p-x-20">
    <a href="/" class="btn btn-ghost text-xl">Головна</a>
    <ul class="list bg-base-100 rounded-box shadow-md">
      <li class="p-4 pb-2 text-xs opacity-60 tracking-wide">Список товорів у кошику</li>

      <li v-for="product in localStorageCartProducts" :key="product.id" class="list-row">
        <div>
          <img
            style="width: 100%; max-width: 150px"
            :src="product.images[0]"
            :alt="product.title"
          />
        </div>
        <div>
          <div>{{ product.title }}</div>
          <div class="text-xs uppercase font-semibold opacity-60">{{ product.description }}</div>
        </div>
        <button class="btn btn-square" @click="countMinus(product)">
          <svg class="w-6 h-6" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path
              opacity="0.5"
              d="M2 12C2 7.28595 2 4.92893 3.46447 3.46447C4.92893 2 7.28595 2 12 2C16.714 2 19.0711 2 20.5355 3.46447C22 4.92893 22 7.28595 22 12C22 16.714 22 19.0711 20.5355 20.5355C19.0711 22 16.714 22 12 22C7.28595 22 4.92893 22 3.46447 20.5355C2 19.0711 2 16.714 2 12Z"
              stroke="#ffffff"
              stroke-width="1.5"
            />
            <path d="M15 12H9" stroke="#ffffff" stroke-width="1.5" stroke-linecap="round" />
          </svg>
        </button>
        <div class="font-bolt"><input class="w-10 p-1" type="number" :value="product.count" /></div>

        <button class="btn btn-square" @click="countPlus(product)">
          <svg class="w-6 h-6" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path
              d="M9 12H15"
              stroke="#ffffff"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
            <path
              d="M12 9L12 15"
              stroke="#ffffff"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
            <path
              d="M3 12C3 4.5885 4.5885 3 12 3C19.4115 3 21 4.5885 21 12C21 19.4115 19.4115 21 12 21C4.5885 21 3 19.4115 3 12Z"
              stroke="#ffffff"
              stroke-width="2"
            />
          </svg>
        </button>
        <div class="font-bolt">{{ (product.price * product.count).toFixed(2) }}$</div>
      </li>
    </ul>
    <div v-if="localStorageCartProducts.length" class="flex flex-col gap-2">
      <input v-model="userName" type="text" placeholder="Введіть ваше ім'я" class="input" />
      <label class="input validator">
        <svg class="h-[1em] opacity-50" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16">
          <g fill="none">
            <path
              d="M7.25 11.5C6.83579 11.5 6.5 11.8358 6.5 12.25C6.5 12.6642 6.83579 13 7.25 13H8.75C9.16421 13 9.5 12.6642 9.5 12.25C9.5 11.8358 9.16421 11.5 8.75 11.5H7.25Z"
              fill="currentColor"
            ></path>
            <path
              fill-rule="evenodd"
              clip-rule="evenodd"
              d="M6 1C4.61929 1 3.5 2.11929 3.5 3.5V12.5C3.5 13.8807 4.61929 15 6 15H10C11.3807 15 12.5 13.8807 12.5 12.5V3.5C12.5 2.11929 11.3807 1 10 1H6ZM10 2.5H9.5V3C9.5 3.27614 9.27614 3.5 9 3.5H7C6.72386 3.5 6.5 3.27614 6.5 3V2.5H6C5.44771 2.5 5 2.94772 5 3.5V12.5C5 13.0523 5.44772 13.5 6 13.5H10C10.5523 13.5 11 13.0523 11 12.5V3.5C11 2.94772 10.5523 2.5 10 2.5Z"
              fill="currentColor"
            ></path>
          </g>
        </svg>
        <input
          v-model="userPhone"
          type="tel"
          class="tabular-nums"
          required
          placeholder="Ваш телефон"
          pattern="[0-9]*"
          minlength="10"
          maxlength="10"
          title="Must be 10 digits"
        />
      </label>
      <p class="validator-hint">Must be 10 digits</p>

      <div class="flex justify-end mt-5">
        <button class="btn btn-primary" @click="createOrder">Оформити замовлення</button>
      </div>
    </div>
    <div v-else>Корзина порожня</div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
let localStorageCartProducts = ref([])
let localProducts = localStorage.getItem('cart')
let userName = ref('')
let userPhone = ref('')

if (localProducts) {
  localStorageCartProducts.value = JSON.parse(localProducts)
}

function countPlus(product) {
  product.count += 1
  localStorage.setItem('cart', JSON.stringify(localStorageCartProducts.value))
}
function countMinus(product) {
  if (product.count > 1) product.count -= 1
  localStorage.setItem('cart', JSON.stringify(localStorageCartProducts.value))
}

function createOrder() {
  const orders = localStorage.getItem('orders')

  if (orders) {
    localStorage.setItem(
      'orders',
      JSON.stringify([
        ...JSON.parse(orders),
        { name: userName.value, phone: userPhone.value, order: localStorageCartProducts.value },
      ]),
    )
  } else {
    localStorage.setItem(
      'orders',
      JSON.stringify([
        { name: userName.value, phone: userPhone.value, order: localStorageCartProducts.value },
      ]),
    )
  }
  localStorage.removeItem('cart')
  localStorageCartProducts.value = []
}
</script>
