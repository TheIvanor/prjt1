<template>
  <div class="p-x-20">
    <ul class="list bg-base-100 rounded-box shadow-md">
      <li class="p-4 pb-2 text-xs opacity-60 tracking-wide">Список товорів у кошику</li>

      <li v-for="product in localProducts" :key="product.id" class="list-row">
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
        <div class="font-bolt"><input type="number" :value="product.count" /></div>

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
    <div class="flex justify-end mt-5">
      <button class="btn btn-primary">Оформити замовлення</button>
    </div>
  </div>
</template>

<script setup>
let localProducts = localStorage.getItem('cart')
if (localProducts) {
  localProducts = JSON.parse(localProducts)
}
</script>
