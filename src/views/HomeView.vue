<script setup>
import { ref, computed, onMounted } from 'vue'
let products = ref([])

const cartProducts = ref([])

const totoalPrise = computed(() => {
  return cartProducts.value
    .reduce((totoal, product) => totoal + product.price * product.count, 0)
    .toFixed(2)
})

const totoalCountProductsInCart = computed(() => {
  return cartProducts.value.reduce((totoal, product) => totoal + product.count, 0)
})

const addToCart = (product) => {
  const findIndex = cartProducts.value.findIndex((item) => item.id === product.id)

  if (findIndex !== -1) {
    cartProducts.value[findIndex].count += 1
  } else {
    cartProducts.value.push(product)
  }

  localStorage.setItem('cart', JSON.stringify(cartProducts.value))
}

onMounted(() => {
  const localProducts = localStorage.getItem('cart')
  if (localProducts) {
    cartProducts.value = JSON.parse(localProducts)
  }
})

fetch('https://dummyjson.com/products')
  .then((res) => res.json())
  .then((data) => {
    products.value = data.products.map((product) => {
      product.count = 1
      return product
    })
  })
</script>

<template>
  <dev>
    <div class="navbar bg-primary text-pimary-content shadow-sm">
      <div class="flex-1">
        <a class="btn btn-ghost text-xl">daisyUI</a>
      </div>
      <div class="flex-none">
        <div class="dropdown dropdown-end">
          <div tabindex="0" role="button" class="btn btn-ghost btn-circle">
            <div class="indicator">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-5 w-5"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z"
                />
              </svg>
              <span class="badge badge-sm indicator-item">{{ totoalCountProductsInCart }}</span>
            </div>
          </div>
          <div
            tabindex="0"
            class="card card-compact dropdown-content bg-base-100 z-[1] mt-3 w-52 shadow"
          >
            <div class="card-body">
              <span class="text-lg font-bold">{{ totoalCountProductsInCart }} товарів</span>
              <span class="text-info">Сума: ${{ totoalPrise }}</span>
              <div class="card-actions">
                <a href="/cart" class="btn btn-primary btn-block">В корзину</a>
              </div>
            </div>
          </div>
        </div>
        <div class="dropdown dropdown-end">
          <div tabindex="0" role="button" class="btn btn-ghost btn-circle avatar">
            <div class="w-10 rounded-full">
              <img
                alt="Tailwind CSS Navbar component"
                src="https://img.daisyui.com/images/stock/photo-1534528741775-53994a69daeb.webp"
              />
            </div>
          </div>
          <ul
            tabindex="0"
            class="menu menu-sm dropdown-content bg-base-100 rounded-box z-[1] mt-3 w-52 p-2 shadow"
          >
            <li>
              <a class="justify-between">
                Profile
                <span class="badge">New</span>
              </a>
            </li>
            <li><a>Settings</a></li>
            <li><a>Logout</a></li>
          </ul>
        </div>
      </div>
    </div>

    <div class="flex flex-wrap gap-4 mx-auto conteiner">
      <div v-for="product in products" :key="product.id" class="card bg-base-100 w-80 shadow-xl">
        <figure>
          <img
            style="width: 100%; max-width: 150px"
            :src="product.images[0]"
            :alt="product.title"
          />
        </figure>
        <div class="card-body">
          <p class="text-center font-bold text-lg">{{ product.price }}</p>
          <h2 class="card-title">{{ product.title }}</h2>
          <p>{{ product.description }}</p>
          <div class="card-actions justify-end">
            <button class="btn btn-primary" @click="addToCart(product)">В корзину</button>
          </div>
        </div>
      </div>
    </div>
  </dev>
</template>
